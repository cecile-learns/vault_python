# from_tensor_slices

## Quand l'utiliser ?

Créer un Dataset depuis des tableaux alignés.

## Pattern

```python
import tensorflow as tf
x=tf.range(20)
y=x%2
dataset=tf.data.Dataset.from_tensor_slices((x,y))
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `tensors` | structure découpée sur l'axe 0 |

## Points de vigilance

- Toutes les composantes ont la même première dimension.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/data/Dataset#from_tensor_slices)
