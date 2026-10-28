# Dense et couche de sortie

## Quand l'utiliser ?

Adapter la dernière couche au type de cible.

## Pattern

```python
import tensorflow as tf
binaire=tf.keras.layers.Dense(1,activation="sigmoid")
multiclasse=tf.keras.layers.Dense(5,activation="softmax")
regression=tf.keras.layers.Dense(1)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `units` | dimension de sortie |
| `activation` | transformation |

## Points de vigilance

- Aligner activation, forme de y et perte.

## Documentation officielle

- [Référence](https://keras.io/api/layers/core_layers/dense/)
