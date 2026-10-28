# tf.matmul et broadcasting

## Quand l'utiliser ?

Effectuer un produit matriciel et comprendre l'extension automatique des dimensions.

## Pattern

```python
import tensorflow as tf

A=tf.constant([[1.0,2.0],[3.0,4.0]])
B=tf.constant([[1.0],[2.0]])
produit=tf.matmul(A,B)
decale=A+tf.constant([10.0,20.0])
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `tf.matmul` | produit matriciel |
| `broadcasting` | étend implicitement une forme compatible |

## Points de vigilance

- Le produit matriciel exige des dimensions internes compatibles.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/linalg/matmul)
