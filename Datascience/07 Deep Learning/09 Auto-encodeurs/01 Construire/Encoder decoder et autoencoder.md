# Encoder, decoder et autoencoder

## Syntaxe

```python
from tensorflow import keras
from tensorflow.keras import layers, Model

dimension_entree = X_train.shape[1]
dimension_latente = 2

entree = keras.Input(shape=(dimension_entree,))
latent = layers.Dense(dimension_latente, activation="relu")(entree)
encoder = Model(entree, latent, name="encoder")

entree_latente = keras.Input(shape=(dimension_latente,))
reconstruction = layers.Dense(dimension_entree, activation="sigmoid")(entree_latente)
decoder = Model(entree_latente, reconstruction, name="decoder")

autoencoder = Model(entree, decoder(encoder(entree)), name="autoencoder")
autoencoder.compile(optimizer="adam", loss="binary_crossentropy")
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `dimension_latente` | taille de la représentation comprimée | plus petite = compression plus forte |
| unités des couches `Dense` | capacité de l'encoder et du decoder | augmenter pour une relation plus complexe |
| activation latente | non-linéarité de l'encodage | `relu` est le choix du cours |
| activation de sortie | domaine des reconstructions | `sigmoid` si les entrées sont normalisées entre 0 et 1 |
| `loss` | mesure de reconstruction | cohérente avec le type et l'échelle des données |

## Vigilances & tips

La forme de sortie du decoder doit être identique à la forme d'entrée à reconstruire.
