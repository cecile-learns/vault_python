# Classification d'images avec un CNN

## Objectif

Préparer les images et entraîner un CNN complet.

## Pas à pas

1. Décoder et redimensionner.
2. Créer les datasets et batches.
3. Augmenter le train.
4. Construire le CNN.
5. Comparer train et validation.

## Pattern complet

```python
from tensorflow import keras
from tensorflow.keras import layers

# 1. CONSTRUIRE LE MODÈLE
model = keras.Sequential([

    # ENTRÉE : dimensions des images
    layers.Input(shape=(32, 32, 3)),

    # EXTRACTION DES CARACTÉRISTIQUES — BLOC 1
    layers.Conv2D(32, (3, 3), activation="relu", padding="same"),
    layers.MaxPooling2D((2, 2)),

    # EXTRACTION DES CARACTÉRISTIQUES — BLOC 2
    layers.Conv2D(64, (3, 3), activation="relu", padding="same"),
    layers.MaxPooling2D((2, 2)),

    # EXTRACTION DES CARACTÉRISTIQUES — BLOC 3
    layers.Conv2D(128, (3, 3), activation="relu", padding="same"),
    layers.MaxPooling2D((2, 2)),

    # TRANSFORMATION DES CARACTÉRISTIQUES EN VECTEUR
    layers.GlobalAveragePooling2D(),

    # CLASSIFICATION
    layers.Dense(100, activation="softmax")
])

# 2. CONFIGURER L'APPRENTISSAGE
model.compile(
    optimizer="adam",
    loss="categorical_crossentropy",
    metrics=["accuracy"]
)

# 3. ENTRAÎNER
history = model.fit(
    X_train_norm,
    y_train_cat,
    epochs=20,
    batch_size=64,
    validation_split=0.2
)

# 4. ÉVALUER SUR LES DONNÉES TEST
test_loss, test_accuracy = model.evaluate(
    X_test_norm,
    y_test_cat
)
```

## Contrôles

- Séparation par individu si nécessaire.
- Formes et pixels contrôlés.
- Matrice de confusion en complément.

## Documentation officielle

- [Référence](https://keras.io/api/layers/convolution_layers/convolution2d/)
![[Pasted image 20261027085659.png]]