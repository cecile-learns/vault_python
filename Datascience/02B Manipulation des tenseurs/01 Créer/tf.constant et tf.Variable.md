# tf.constant et tf.Variable

## Quand l'utiliser ?

Créer une donnée fixe ou un état modifiable appris.

## Pattern

```python
import tensorflow as tf
x=tf.constant([[1.0,2.0],[3.0,4.0]])
w=tf.Variable([[0.5],[1.0]],trainable=True)
y=tf.matmul(x,w)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `dtype` | type numérique |
| `shape` | forme |
| `trainable` | variable optimisée |

## Points de vigilance

- Une constante n'est pas mise à jour.
- Contrôler shape et dtype.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/constant)
