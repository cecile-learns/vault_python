# Data augmentation

## Quand l'utiliser ?

Créer des variations plausibles pendant l'entraînement.

## Pattern

```python
import tensorflow as tf
augmentation=tf.keras.Sequential([
 tf.keras.layers.RandomFlip("horizontal"),
 tf.keras.layers.RandomRotation(0.1),
 tf.keras.layers.RandomZoom(0.1)])
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `factor` | amplitude |
| `fill_mode` | remplissage |

## Points de vigilance

- Préserver le sens des classes.
- Appliquer au train uniquement.

## Documentation officielle

- [Référence](https://keras.io/api/layers/preprocessing_layers/image_augmentation/)
