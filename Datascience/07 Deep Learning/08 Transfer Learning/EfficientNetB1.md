# EfficientNetB1

## Quand l'utiliser ?

Utiliser un extracteur compact préentraîné.

## Pattern

```python
import tensorflow as tf
base=tf.keras.applications.EfficientNetB1(weights="imagenet",include_top=False,input_shape=(240,240,3))
base.trainable=False
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `pooling` | avg ou max |
| `classes` | si include_top=True |

## Points de vigilance

- Respecter la taille d'image conseillée.

## Documentation officielle

- [Référence](https://keras.io/api/applications/efficientnet/efficientnet_models/)
