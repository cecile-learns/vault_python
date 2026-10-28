# Indexer un tenseur

## Quand l'utiliser ?

Sélectionner lignes, colonnes ou canaux.

## Pattern

```python
import tensorflow as tf
x=tf.constant([[1,2,3],[4,5,6]])
premiere_ligne=x[0]
deux_colonnes=x[:,:2]
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `:` | tous les éléments d'un axe |
| `a:b` | tranche de a inclus à b exclu |

## Points de vigilance

- La première dimension est souvent le batch.

## Documentation officielle

- [Référence](https://www.tensorflow.org/guide/tensor_slicing)
