# Transfer Learning puis fine-tuning

## Objectif

Adapter EfficientNet sans détruire ses poids utiles.

## Pas à pas

1. Charger sans la tête.
2. Geler et entraîner une nouvelle tête.
3. Évaluer.
4. Libérer les dernières couches.
5. Recompiler avec un faible taux.

## Pattern complet

```python
import tensorflow as tf
base=tf.keras.applications.EfficientNetB1(weights="imagenet",include_top=False,input_shape=(240,240,3))
base.trainable=False
inputs=tf.keras.Input((240,240,3))
x=base(inputs,training=False)
x=tf.keras.layers.GlobalAveragePooling2D()(x)
outputs=tf.keras.layers.Dense(4,activation="softmax")(x)
model=tf.keras.Model(inputs,outputs)
model.compile(optimizer=tf.keras.optimizers.Adam(1e-3),loss="sparse_categorical_crossentropy",metrics=["accuracy"])
base.trainable=True
for layer in base.layers[:-20]: layer.trainable=False
model.compile(optimizer=tf.keras.optimizers.Adam(1e-5),loss="sparse_categorical_crossentropy",metrics=["accuracy"])
```

## Contrôles

- Prétraitement compatible.
- Recompilation après trainable.
- Taux de fine-tuning très faible.

## Documentation officielle

- [Référence](https://keras.io/guides/transfer_learning/)
