# Réduire la dimension avec un auto-encodeur

## Pour faire quoi ?

Apprendre une représentation latente non linéaire puis l'utiliser pour visualiser ou modéliser les données.

## Script réutilisable

```python
import numpy as np
from tensorflow import keras
from tensorflow.keras import layers, Model

X_train_norm = X_train.astype("float32") / 255.0
X_test_norm = X_test.astype("float32") / 255.0
X_train_flat = X_train_norm.reshape(len(X_train_norm), -1)
X_test_flat = X_test_norm.reshape(len(X_test_norm), -1)

dimension_entree = X_train_flat.shape[1]
dimension_latente = 2

entree = keras.Input(shape=(dimension_entree,))
x = layers.Dense(128, activation="relu")(entree)
latent = layers.Dense(dimension_latente, name="latent")(x)
encoder = Model(entree, latent)

x = layers.Dense(128, activation="relu")(latent)
sortie = layers.Dense(dimension_entree, activation="sigmoid")(x)
autoencoder = Model(entree, sortie)

autoencoder.compile(optimizer="adam", loss="binary_crossentropy")
history = autoencoder.fit(
    X_train_flat,
    X_train_flat,
    epochs=30,
    batch_size=256,
    validation_split=0.2,
    verbose=1,
)

X_test_latent = encoder.predict(X_test_flat, batch_size=256, verbose=0)
```

## Paramètres et hyperparamètres

| Paramètre | Effet |
|---|---|
| `dimension_latente` | degré de compression et taille de la représentation |
| unités intermédiaires | capacité de l'encoder |
| `epochs` | durée de l'apprentissage |
| `batch_size` | mémoire, vitesse et stabilité |
| `validation_split` | suivi de la généralisation |

## Vigilances & tips

Ne pas comparer directement la représentation latente à PCA ou UMAP sans tenir compte de leur objectif et de leur apprentissage différents.
