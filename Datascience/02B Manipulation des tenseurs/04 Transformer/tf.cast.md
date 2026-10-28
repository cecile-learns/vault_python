# tf.cast

## Quand l'utiliser ?

Changer le type numérique d'un tenseur.

## Pattern

```python
import tensorflow as tf

x=tf.constant([1,2,3])
y=tf.cast(x,tf.float32)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `x` | tenseur source |
| `dtype` | type cible |

## Points de vigilance

- Une conversion vers un entier peut perdre la partie décimale.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/cast)
