# NumPy et TensorFlow

## Quand l'utiliser ?

Passer d'un array NumPy à un tenseur et inversement.

## Pattern

```python
import numpy as np
import tensorflow as tf

array=np.array([1,2,3],dtype=np.float32)
tenseur=tf.convert_to_tensor(array)
retour=tenseur.numpy()
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `tf.convert_to_tensor` | convertit une valeur compatible |
| `tensor.numpy()` | retourne un array en exécution immédiate |

## Points de vigilance

- numpy() n'est pas disponible de la même manière dans tous les graphes compilés.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/convert_to_tensor)
