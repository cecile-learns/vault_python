# tf.function

## Quand l'utiliser ?

Transformer une fonction TensorFlow en graphe exécutable.

## Pattern

```python
import tensorflow as tf

@tf.function
def prediction(x,w,b):
    return tf.matmul(x,w)+b
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `input_signature` | contraint formes et types |

## Points de vigilance

- Éviter les effets de bord Python.
- Déboguer d’abord sans décoration.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/function)
