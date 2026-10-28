# expand_dims et squeeze

## Quand l'utiliser ?

Ajouter ou retirer un axe de taille 1.

## Pattern

```python
import tensorflow as tf
image=tf.zeros((28,28))
avec_canal=tf.expand_dims(image,axis=-1)
sans_canal=tf.squeeze(avec_canal,axis=-1)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `axis` | position de l'axe |

## Points de vigilance

- squeeze exige une dimension égale à 1.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/expand_dims)
