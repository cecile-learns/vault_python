# ResNet50 préentraîné

## Quand l'utiliser ?

Réutiliser un extracteur ImageNet sans sa tête.

## Pattern

```python
import tensorflow as tf
base=tf.keras.applications.ResNet50(weights="imagenet",include_top=False,input_shape=(224,224,3))
base.trainable=False
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `weights` | poids |
| `include_top` | tête originale |
| `input_shape` | forme |

## Points de vigilance

- Appliquer le prétraitement attendu.

## Documentation officielle

- [Référence](https://keras.io/api/applications/resnet/)
