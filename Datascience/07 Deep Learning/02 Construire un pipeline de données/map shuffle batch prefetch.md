# map shuffle batch prefetch

## Quand l'utiliser ?

Transformer, mélanger, regrouper et précharger.

## Pattern

```python
import tensorflow as tf
dataset=tf.data.Dataset.range(100)
dataset=dataset.shuffle(100,seed=42)
dataset=dataset.map(lambda x:tf.cast(x,tf.float32)/99.0)
dataset=dataset.batch(16).prefetch(tf.data.AUTOTUNE)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `buffer_size` | réservoir de mélange |
| `batch_size` | exemples par mise à jour |
| `num_parallel_calls` | parallélisme |

## Points de vigilance

- Mélanger le train, pas le test.
- Contrôler l'ordre des transformations.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/data/Dataset)
