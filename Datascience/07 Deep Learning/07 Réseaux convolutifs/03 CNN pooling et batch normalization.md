
## Pattern

```python
import tensorflow as tf
layers.BatchNormalization()

model = keras.Sequential([
    layers.Input(shape=(32, 32, 3)),

    # BLOC 1
    layers.Conv2D(32, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),

    # BLOC 2
    layers.Conv2D(64, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),

    # BLOC 3
    layers.Conv2D(128, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),

    # TRANSITION VERS LA CLASSIFICATION
    layers.Flatten(),

    # CLASSIFICATION
    layers.Dense(256, activation="relu"),
    layers.BatchNormalization(),
    layers.Dense(100, activation="softmax")
])
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `Add` | addition élément par élément |
| `filters` | compatible avec le raccourci |

## Points de vigilance

- Projeter le raccourci si les formes diffèrent.

## Documentation officielle

- [Référence](https://keras.io/api/layers/merging_layers/add/)
