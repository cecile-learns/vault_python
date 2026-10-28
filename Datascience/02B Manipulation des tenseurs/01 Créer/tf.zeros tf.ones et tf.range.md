# tf.zeros tf.ones et tf.range

## Quand l'utiliser ?

Créer rapidement des tenseurs remplis ou une séquence.

## Pattern

```python
import tensorflow as tf

zeros=tf.zeros((2,3),dtype=tf.float32)
ones=tf.ones((2,3),dtype=tf.float32)
valeurs=tf.range(start=0,limit=10,delta=2)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `shape` | forme de zeros et ones |
| `dtype` | type des valeurs |
| `start, limit, delta` | début, borne exclue et pas |

## Points de vigilance

- tf.range exclut la borne limit.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/range)
