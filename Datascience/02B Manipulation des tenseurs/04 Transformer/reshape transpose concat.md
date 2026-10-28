# reshape transpose concat

## Quand l'utiliser ?

Réorganiser ou assembler des tenseurs.

## Pattern

```python
import tensorflow as tf
x=tf.range(12)
x2=tf.reshape(x,(3,4))
xt=tf.transpose(x2)
y=tf.concat([x2,x2],axis=0)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `shape` | nouvelle forme |
| `perm` | ordre des axes |
| `axis` | axe d'assemblage |

## Points de vigilance

- reshape conserve le nombre d'éléments.
- Les autres axes doivent être compatibles.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/reshape)
