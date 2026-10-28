# Geler puis fine-tuner

## Quand l'utiliser ?

Entraîner la tête puis ajuster une partie du backbone.

## Pattern

```python
import tensorflow as tf
base=tf.keras.applications.ResNet50(weights="imagenet",include_top=False,input_shape=(224,224,3))
base.trainable=False
inputs=tf.keras.Input((224,224,3))
x=base(inputs,training=False)
x=tf.keras.layers.GlobalAveragePooling2D()(x)
outputs=tf.keras.layers.Dense(3,activation="softmax")(x)
model=tf.keras.Model(inputs,outputs)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `trainable` | gèle ou libère |
| `training=False` | stabilise BatchNormalization |

## Points de vigilance

- Recompiler après changement de trainable.
- Fine-tuner avec un très faible taux.

## Documentation officielle

- [Référence](https://keras.io/guides/transfer_learning/)
