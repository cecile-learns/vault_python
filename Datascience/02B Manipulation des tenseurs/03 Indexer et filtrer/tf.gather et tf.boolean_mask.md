# tf.gather et tf.boolean_mask

## Quand l'utiliser ?

Sélectionner des positions ou filtrer avec un masque booléen.

## Pattern

```python
import tensorflow as tf

x=tf.constant([10,20,30,40])
selection=tf.gather(x,[0,2])
filtre=tf.boolean_mask(x,x>20)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `indices` | positions sélectionnées par gather |
| `axis` | axe concerné |
| `mask` | tenseur booléen compatible |

## Points de vigilance

- gather sélectionne des indices; boolean_mask conserve les valeurs vraies.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/boolean_mask)
