
## Pattern DropOut

```python
import tensorflow as tf
layers.BatchNormalization()

model = keras.Sequential([
    layers.Input(shape=(32, 32, 3)),

    # BLOC 1
    layers.Conv2D(32, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),
    layers.Dropout(0.25),  # Désactive 25 % des unités

    # BLOC 2
    layers.Conv2D(64, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),
    layers.Dropout(0.25),

    # BLOC 3
    layers.Conv2D(128, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),
    layers.Dropout(0.25),

    # TRANSITION
    layers.Flatten(),

    # CLASSIFICATION
    layers.Dense(256, activation="relu"),
    layers.BatchNormalization(),
    layers.Dropout(0.5),  # Désactive 50 % des unités

    layers.Dense(100, activation="softmax")
])
```


## Paramètres et éléments utiles

![[Pasted image 20261027090426.png]]



## Pattern Early-stop

```python
import tensorflow as tf

model.compile(
    optimizer="adam",
    loss="categorical_crossentropy",
    metrics=["accuracy"]
)

from tensorflow.keras import callbacks

early_stop = callbacks.EarlyStopping(
    monitor="val_loss",
    patience=7,
    min_delta=0.001,
    restore_best_weights=True
)
history = model.fit(
    X_train_norm,
    y_train_cat,
    epochs=80,
    batch_size=64,
    validation_split=0.2,
    callbacks=[early_stop],
    verbose=1
)
```


### Evaluer après l'entrainement
```Python
test_loss, test_accuracy = model.evaluate(
    X_test_norm,
    y_test_cat,
    verbose=0
)

print(f"Test accuracy : {test_accuracy:.3f}")
print(f"Test loss : {test_loss:.3f}")
```


```python 
import matplotlib.pyplot as plt
plt.plot(history.history['loss'], label="Train loss")
plt.plot(history.history['val_loss'], label="Validation loss")

plt.xlabel("Epochs")
plt.ylabel("Loss")
plt.title("Evolution de la loss sur l'ensemble d'entraînement et de test au fil des epochs")
plt.legend()
plt.show()
```
![[Pasted image 20261027101819.png]]