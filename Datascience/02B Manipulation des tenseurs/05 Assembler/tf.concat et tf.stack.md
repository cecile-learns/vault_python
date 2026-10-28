# tf.concat et tf.stack

## Quand l'utiliser ?

Assembler des tenseurs sur un axe existant ou sur un nouvel axe.

## Pattern

```python
import tensorflow as tf

a=tf.constant([1,2])
b=tf.constant([3,4])
concatene=tf.concat([a,b],axis=0)
empile=tf.stack([a,b],axis=0)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `tf.concat` | utilise un axe existant |
| `tf.stack` | crée un nouvel axe |
| `axis` | position de l'assemblage |

## Points de vigilance

- Les formes doivent être compatibles sur les axes non assemblés.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/concat)
