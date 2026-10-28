# Dropout et BatchNormalization

## Quand l'utiliser ?

Régulariser les activations et stabiliser l'entraînement.

## Pattern

```python
import tensorflow as tf
dropout=tf.keras.layers.Dropout(0.3)
batchnorm=tf.keras.layers.BatchNormalization(momentum=0.99,epsilon=0.001)



from tensorflow.keras.layers import Dropout

Dropout(0.5)  # 50 % des neurones désactivés pendant l'entraînement

```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `rate` | fraction désactivée |
| `momentum` | statistiques mobiles |
| `epsilon` | stabilité |

## Points de vigilance

- Le comportement change entre entraînement et inférence.

## Documentation officielle

- [Référence](https://keras.io/api/layers/regularization_layers/)
