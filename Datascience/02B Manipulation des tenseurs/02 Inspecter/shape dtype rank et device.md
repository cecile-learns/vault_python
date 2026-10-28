# shape dtype rank et device

## Quand l'utiliser ?

Contrôler la structure, le type et l'emplacement d'un tenseur.

## Pattern

```python
import tensorflow as tf

x=tf.constant([[1.0,2.0],[3.0,4.0]])
forme=x.shape
type_valeurs=x.dtype
n_dimensions=tf.rank(x)
emplacement=x.device
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `shape` | taille de chaque axe |
| `dtype` | type numérique |
| `tf.rank` | nombre d'axes |
| `device` | CPU ou accélérateur utilisé |

## Points de vigilance

- Une dimension peut être inconnue dans un graphe dynamique.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/Tensor)
