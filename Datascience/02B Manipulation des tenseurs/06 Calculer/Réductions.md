# Réductions

## Quand l'utiliser ?

Résumer un tenseur sur un axe.

## Pattern

```python
import tensorflow as tf
x=tf.constant([[1.0,2.0],[3.0,4.0]])
moyenne=tf.reduce_mean(x,axis=0)
somme=tf.reduce_sum(x)
maximum=tf.reduce_max(x)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `axis` | axe réduit |
| `keepdims` | conserve la dimension |

## Points de vigilance

- Une réduction change souvent la forme.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/math)
