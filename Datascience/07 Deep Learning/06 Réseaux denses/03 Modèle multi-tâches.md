# Modèle multi-tâches

## Objectif

Partager un tronc et optimiser plusieurs sorties.

## Pas à pas

1. Définir chaque cible.
2. Construire le tronc commun.
3. Nommer les têtes.
4. Compiler avec des dictionnaires.
5. Suivre chaque tâche.

## Pattern complet

```python
target_columns = ['class', 'toxicity-level']
X = df.drop(columns=target_columns)
y = df[target_columns]

# On sépare d’abord le test (15%)
X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.15,
    random_state=42,
    stratify=y['class']
)

# On sépare la validation
# (15% du total, soit environ 18% du train initial)
X_train, X_val, y_train, y_val = train_test_split(
    X_train, y_train,
    test_size=0.18,  # = 0.15 / 0.85 pour obtenir 15% du total
    random_state=42,
    stratify=y_train['class']
)

# Encode target variable
label_encoder = LabelEncoder()
y_train['class'] = label_encoder.fit_transform(y_train['class'])
y_val['class'] = label_encoder.transform(y_val['class'])
y_test['class'] = label_encoder.transform(y_test['class'])

# OneHotEncode features
onehot_encoder = OneHotEncoder(sparse_output=False, handle_unknown='ignore')
X_train = onehot_encoder.fit_transform(X_train)
X_val = onehot_encoder.transform(X_val)
X_test = onehot_encoder.transform(X_test)
```
### Architecture multitâche
Keras permet d'implémenter un unique réseau de neurones pour prédire deux variables. Plutôt que d'entraîner deux modèles séparés, cela permet d'économiser du temps de calcul et des ressources computationnelles. Seulement, les résultats sont légèrement inférieurs par rapport aux modèle mono tache.
```python
inputs = Input(shape=(X_train.shape[1],))


x = layers.Dense(128, activation='relu')(inputs)
x = layers.Dense(64, activation='relu')(x)
x = layers.Dense(32, activation='relu')(x)


class_head = layers.Dense(1, activation='sigmoid', name='class_head')(x)
toxicity_head = layers.Dense(1, activation='sigmoid', name='toxicity_head')(x)

model = Model(inputs=inputs, outputs=[class_head, toxicity_head])
```

### Fonction de perte combinée

Lors de la compilation du modèle, il est nécessaire d'indiquer une fonction de perte à optimiser. Étant donné que nous cherchons à optimiser 2 variables (1 catégorielle et 1 numérique) en même temps, une simple `binary_crossentropy` ou `mse` ne suffirait pas.

Il est donc nécessaire de créer une **fonction de perte combinée**. IL est possible de les sommer ou d'ajouter un coefficients à chaque fonction de perte.  
Attention toujours faire attention à l'échelle des variables.  
```Python
loss_weights = {
    'class_head': 0.7, 
    'toxicity_head': 0.3
}

losses = {
    'class_head': 'binary_crossentropy',
    'toxicity_head': 'mean_squared_error'
}
metrics={
        'class_head': 'accuracy', 
        'toxicity_head': 'mae'
    }
```

### Compilation

```python
model.compile(
    optimizer=Adam(),
    loss=losses,
    loss_weights=loss_weights,
    metrics=metrics
)
```

### Entrainement
```python
y_train_dict = {
    'class_head': y_train['class'].values,
    'toxicity_head': y_train['toxicity-level'].values
}

y_val_dict = {
    'class_head': y_val['class'].values,
    'toxicity_head': y_val['toxicity-level'].values
}

y_test_dict = {
    'class_head': y_test['class'].values,
    'toxicity_head': y_test['toxicity-level'].values
}

history = model.fit(
    X_train, y_train_dict,
    validation_data=(X_val, y_val_dict),
    epochs=25,
    batch_size=16
)
```
### Afficher l'évolution de la perte et des 2 métriques
```python
metrics = history.history
epochs = range(1, len(metrics['loss']) + 1)

fig, (ax1, ax2, ax3) = plt.subplots(1, 3, figsize=(20, 5))

# --- Graphique 1 : Global Loss (Pondérée) ---
ax1.plot(epochs, metrics['loss'], 'b-', label='Train Global Loss')
ax1.plot(epochs, metrics['val_loss'], 'r--', label='Val Global Loss')
ax1.set_title('Global Loss Evolution')
ax1.set_xlabel('Epochs')
ax1.set_ylabel('Loss')
ax1.legend()
ax1.grid(True)

# --- Graphique 2 : Classification (Accuracy) ---
# Keras nomme la clé : nom_couche + '_' + nom_métrique
ax2.plot(epochs, metrics['class_head_accuracy'], 'b-', label='Train Accuracy')
ax2.plot(epochs, metrics['val_class_head_accuracy'], 'r--', label='Val Accuracy')
ax2.set_title('Classification Head (Accuracy)')
ax2.set_xlabel('Epochs')
ax2.set_ylabel('Accuracy')
ax2.legend()
ax2.grid(True)

# --- Graphique 3 : Toxicity (MAE) ---
ax3.plot(epochs, metrics['toxicity_head_mae'], 'b-', label='Train MAE')
ax3.plot(epochs, metrics['val_toxicity_head_mae'], 'r--', label='Val MAE')
ax3.set_title('Toxicity Head (MAE)')
ax3.set_xlabel('Epochs')
ax3.set_ylabel('Mean Absolute Error')
ax3.legend()
ax3.grid(True)

plt.tight_layout()
plt.show()
```
![[Pasted image 20261025101051.png]]

### Evaluation et intérprétation

```python
model.evaluate(X_test, y_test_dict)
```
![[Pasted image 20261025101630.png]]

```python
import matplotlib.pyplot as plt
import numpy as np

weights, biases = model.layers[1].get_weights()

plt.figure(figsize=(8, 6))
plt.imshow(weights, aspect='auto')
plt.colorbar()
plt.title("Visualisation des poids - Dense 1")
plt.xlabel("Neurones de sortie")
plt.ylabel("Caractéristiques d'entrée")
plt.show()
```

![[Pasted image 20261025101742.png]]
Chaque ligne correspond à une feature en entrée, et chaque colonne à un neurone de la première couche.

Des couleurs variées indiquent des neurones spécialisés et un modèle actif. Une teinte entièrement gris foncé ou claire indique une absence d'apprentissage ou un problème d'initialisation.





