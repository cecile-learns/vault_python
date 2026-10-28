Le jeu de données `housing.csv` recense différentes caractéristiques de biens immobiliers en Californie dans les années 90. On y trouve des caractéristiques telles que la position du bien, le nombre de pièces, la proximité à l'océan, et notre variable cible, la valeur médiane du bien : `median_house_value`.
```python
import pandas as pd
import numpy as np

df = pd.read_csv("housing.csv")
display(df.head())

display(df.isnull().sum())

# print distribution of target variable
import matplotlib.pyplot as plt
plt.hist(df['median_house_value'], bins=50)
plt.xlabel('Median House Value')
plt.ylabel('Frequency')
plt.title('Distribution of Median House Value')
plt.show()
```
![[Pasted image 20261024120200.png]]
```python
# Valeurs extrêmes
threshold = df["median_house_value"].quantile(0.95)
print("Valeur seuil :", threshold)
df = df[df["median_house_value"] <= threshold]

#Séparation du jeux
from sklearn.model_selection import train_test_split

X = df.drop("median_house_value", axis=1)
y = df["median_house_value"]

# On sépare d’abord le test (15%)
X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.15,
    random_state=42,
)

# On sépare la validation
# (15% du total, soit environ 18% du train initial)
X_train, X_val, y_train, y_val = train_test_split(
    X_train, y_train,
    test_size=0.18,  # = 0.15 / 0.85 pour obtenir 15% du total
    random_state=42,
)

print("Train:", X_train.shape)
print("Val  :", X_val.shape)
print("Test :", X_test.shape)

#Valeurs manquantes
from sklearn.impute import SimpleImputer

imputer = SimpleImputer(strategy="median")
X_train["total_bedrooms"] = imputer.fit_transform(X_train[["total_bedrooms"]])
X_val["total_bedrooms"] = imputer.transform(X_val[["total_bedrooms"]])
X_test["total_bedrooms"] = imputer.transform(X_test[["total_bedrooms"]])

X_train.isnull().sum()


#Encodage
from sklearn.preprocessing import OneHotEncoder

print("Nombre de modalités de ocean_proximity :", X_train["ocean_proximity"].nunique())

ohe = OneHotEncoder(handle_unknown="ignore", sparse_output=False)

ohe.fit(X_train[['ocean_proximity']])

X_train_ohe = ohe.transform(X_train[['ocean_proximity']])
X_val_ohe   = ohe.transform(X_val[['ocean_proximity']])
X_test_ohe  = ohe.transform(X_test[['ocean_proximity']])

ohe_cols = ohe.get_feature_names_out(['ocean_proximity'])

X_train = pd.concat([
    X_train.drop(columns=['ocean_proximity']).reset_index(drop=True),
    pd.DataFrame(X_train_ohe, columns=ohe_cols)
], axis=1)

X_val = pd.concat([
    X_val.drop(columns=['ocean_proximity']).reset_index(drop=True),
    pd.DataFrame(X_val_ohe, columns=ohe_cols)
], axis=1)

X_test = pd.concat([
    X_test.drop(columns=['ocean_proximity']).reset_index(drop=True),
    pd.DataFrame(X_test_ohe, columns=ohe_cols)
], axis=1)

print(X_train.shape, X_val.shape, X_test.shape)


#Standardisation
from sklearn.preprocessing import StandardScaler

X_scaler = StandardScaler()

num_cols = ['longitude', 'latitude', 'housing_median_age', 'total_rooms',
            'total_bedrooms', 'population', 'households', 'median_income']

X_train[num_cols] = X_scaler.fit_transform(X_train[num_cols])
X_val[num_cols] = X_scaler.transform(X_val[num_cols])
X_test[num_cols] = X_scaler.transform(X_test[num_cols])

# Autre scaler pour la variable cible
y_scaler = StandardScaler()

y_train = y_scaler.fit_transform(y_train.values.reshape(-1, 1))

# Transformez y_val et y_test
y_val = y_scaler.transform(y_val.values.reshape(-1, 1))
y_test = y_scaler.transform(y_test.values.reshape(-1, 1))

display(X_train.head())
y_train
```

![[Pasted image 20261024120339.png]]

```python
#Optimisation et régularisation
from tensorflow.keras import Model, layers, Input

inputs = Input(shape=(X_train.shape[1],))

x = layers.Dense(64, activation='relu')(inputs)
x = layers.Dense(64, activation='relu')(x)
x = layers.Dense(32, activation='relu')(x)
x = layers.Dense(16, activation='relu')(x)

outputs = layers.Dense(1, activation='linear')(x)

model = Model(inputs=inputs, outputs=outputs)

model.summary()

```

![[Pasted image 20261024121314.png]]

```python
#Optimiseur & Callbacks
from tensorflow.keras.optimizers import SGD

model.compile(
    optimizer=SGD(learning_rate=1e-3),
    loss='mse',
    metrics=['mae']
)

history = model.fit(
    X_train, y_train,
    validation_data=(X_val, y_val),
    batch_size=32,
    epochs=50,
    verbose=1
)

def plot_history(history):
    # Récupération des valeurs
    loss = history.history['loss']
    val_loss = history.history['val_loss']
    mae = history.history['mae']
    val_mae = history.history['val_mae']

    epochs = range(1, len(loss) + 1)

    plt.figure(figsize=(10, 4))

    # Loss
    plt.subplot(1, 2, 1)
    plt.plot(epochs, loss, label='Train Loss')
    plt.plot(epochs, val_loss, label='Val Loss')
    plt.title('Loss over Epochs')
    plt.xlabel('Epochs')
    plt.ylabel('Loss')
    plt.legend()
    plt.grid(True)

    # MAE
    plt.subplot(1, 2, 2)
    plt.plot(epochs, mae, label='Train MAE')
    plt.plot(epochs, val_mae, label='Val MAE')
    plt.title('MAE over Epochs')
    plt.xlabel('Epochs')
    plt.ylabel('MAE')
    plt.legend()
    plt.grid(True)

    plt.tight_layout()
    plt.show()

plot_history(history)

```

![[Pasted image 20261024121928.png]]

Les performances semblent s'améliorer au fil de l'entraînement, bien que l'échelle étant modifiée, nous ne savons pas dans quelle mesure.

Une remarque que l'on peut se faire est d'observer un motif légèrement en dents de scie sur la MAE à partir de l'epoch 25. Cela est dû à la vitesse d'apprentissage qui est légèrement trop grande en fin d'entraînement, oscillant ainsi autour des meilleurs paramètres sans les atteindre.

L'idéal pour nous serait de pouvoir adapter la vitesse d'apprentissage au fil de l'entraînement, ou, d'une manière générale, de pouvoir adapter l'entraînement du modèle en fonction de ses performances. Il existe pour cela plusieurs options :

- **Learning Rate Schedules** : Le LR diminue selon une règle définie, indépendamment de la performance. Par exemple, diviser le LR (_Learning Rate_) par un facteur toutes les X epochs.
    
- **ReduceLROnPlateau** : LR réduit si la validation stagne. Cela revient à dire : → "Si pas d’amélioration après 10 epochs, LR = LR * 0.5"
    
- **EarlyStopping** : L'entraînement s'arrête si la validation stagne. Cela revient à dire : → "Si pas d’amélioration après 10 epochs, on considère que le meilleur modèle est atteint".
    
- **Adaptative Optimizers** : Le LR adapte individuellement chaque paramètre selon les gradients. Le plus populaire est `Adam`, utilisant adaptation + momentum pour régler les paramètres.
    
- **Gradient Clipping** : De façon indirecte, on stabilise l’apprentissage pour utiliser un LR plus élevé sans divergence.

```Python
#Utilisation d'opimizer Adam
from tensorflow.keras.optimizers import Adam
from tensorflow.keras.callbacks import ReduceLROnPlateau

lr_scheduler = ReduceLROnPlateau(
    monitor='val_loss',
    factor=0.5,
    patience=5,
    verbose=1,
    min_lr=1e-6
)

model.compile(
    optimizer=Adam(learning_rate=1e-3),
    loss='mse',
    metrics=['mae']
)

history = model.fit(
    X_train, y_train,
    validation_data=(X_val, y_val),
    batch_size=32,
    epochs=50,
    callbacks=[lr_scheduler],
    verbose=0
)

plot_history(history)
```

Epoch 32: ReduceLROnPlateau reducing learning rate to 0.0005000000237487257.

Epoch 37: ReduceLROnPlateau reducing learning rate to 0.0002500000118743628.

Epoch 42: ReduceLROnPlateau reducing learning rate to 0.0001250000059371814.

Epoch 47: ReduceLROnPlateau reducing learning rate to 6.25000029685907e-05.

![[Pasted image 20261024122407.png]]

Il y a plusieurs choses à observer sur ces courbes :

- Les performances globales se sont améliorées : la loss atteinte est plus faible que précédemment, et les métriques sont meilleures.
    
- A partir de l'epoch 20, le callback s'applique et le learning rate est réduit : cela stabilise l'entraînement.
    
- Il y a un écart important de performance entre l'ensemble d'entraînement et de validation. Le modèle colle trop aux données d'entraînement et généralise mal : il **overfit**.

```python
#réduire l'offerfitting avec dropout
from tensorflow.keras.layers import Dropout

inputs = Input(shape=(X_train.shape[1],))
x = layers.Dense(64, activation='relu')(inputs)
x = layers.Dropout(0.2)(x)
x = layers.Dense(64, activation='relu')(x)
x = layers.Dropout(0.2)(x)
x = layers.Dense(32, activation='relu')(x)
x = layers.Dropout(0.2)(x)
x = layers.Dense(16, activation='relu')(x)
outputs = layers.Dense(1, activation='linear')(x)

model = Model(inputs=inputs, outputs=outputs)

lr_scheduler = ReduceLROnPlateau(
    monitor='val_loss',
    factor=0.5,
    patience=5,
    verbose=1,
    min_lr=1e-6
)

model.compile(
    optimizer=Adam(learning_rate=1e-3),
    loss='mse',
    metrics=['mae']
)

history = model.fit(
    X_train, y_train,
    validation_data=(X_val, y_val),
    batch_size=32,
    epochs=50,
    callbacks=[lr_scheduler],
    verbose=0
)

plot_history(history)
```

![[Pasted image 20261024122552.png]]