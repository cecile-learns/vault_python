# tf.GradientTape

## Quand l'utiliser ?

Dériver automatiquement une perte par rapport aux poids.

## Pattern

```python
import tensorflow as tf
w=tf.Variable(2.0)
with tf.GradientTape() as tape:
    loss=(w-5.0)**2
gradient=tape.gradient(loss,w)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `persistent=True` | plusieurs calculs de gradients |

## Points de vigilance

- Le calcul doit se faire dans le bloc tape.
- Une opération non différentiable peut produire None.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/GradientTape)
