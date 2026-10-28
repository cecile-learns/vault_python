# summary count_params et history

## Quand l'utiliser ?

Inspecter l’architecture, les paramètres et les courbes.

## Pattern

```python
import tensorflow as tf

model=tf.keras.Sequential([tf.keras.Input((10,)),tf.keras.layers.Dense(8),tf.keras.layers.Dense(1)])
model.summary()
n_params=model.count_params()
# après fit : pertes=history.history["loss"]
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `summary` | formes et paramètres |
| `count_params` | total des paramètres |
| `history.history` | mesures par epoch |

## Points de vigilance

- Comparer train et validation.

## Documentation officielle

- [Référence](https://keras.io/api/models/model/)
