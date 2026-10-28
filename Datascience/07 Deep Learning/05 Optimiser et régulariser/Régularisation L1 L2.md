# Régularisation L1 L2

## Quand l'utiliser ?

Pénaliser les poids trop complexes.

## Pattern

```python
import tensorflow as tf
layer=tf.keras.layers.Dense(64,activation="relu",kernel_regularizer=tf.keras.regularizers.l2(1e-4))


from tensorflow.keras import regularizers
from tensorflow.keras.layers import Dense

Dense(64, activation='relu', kernel_regularizer=regularizers.l2(0.01)) # L2
Dense(64, activation='relu', kernel_regularizer=regularizers.l1(0.01)) # L1

```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `l1` | favorise des poids nuls |
| `l2` | réduit les grandes amplitudes |

## Points de vigilance

- La pénalité s'ajoute à la perte.

## Documentation officielle

- [Référence](https://keras.io/api/layers/regularizers/)
