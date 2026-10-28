# Conv2D

## Quand l'utiliser ?

Extraire des motifs locaux dans une image.

## Pattern

```python
import tensorflow as tf
conv=tf.keras.layers.Conv2D(filters=32,kernel_size=(3,3),strides=1,padding="same",activation="relu")


#cours
cnn_model = models.Sequential([
    # Bloc 1
    layers.Conv2D(32, (3, 3), activation="relu", padding="same", input_shape=(32, 32, 3)),
    layers.MaxPooling2D((2, 2)),
    
    # Bloc 2
    layers.Conv2D(64, (3, 3), activation="relu", padding="same"),
    layers.MaxPooling2D((2, 2)),
    
    # Bloc 3
    layers.Conv2D(128, (3, 3), activation="relu", padding="same"),
    layers.MaxPooling2D((2, 2)),
    
    
# Entraînement du CNN
start_time = time.time()

cnn_history = cnn_model.fit(
    X_train_norm, y_train_cat,
    epochs=15,
    batch_size=64,
    validation_split=0.2,
    verbose=1
)

cnn_train_time = time.time() - start_time
print(f"\nTemps d'entraînement CNN : {cnn_train_time:.1f} secondes")

#Courbes d'apprentissage
plot_history(cnn_history, "CNN")

# Évaluation sur le jeu de test
cnn_loss, cnn_acc = cnn_model.evaluate(X_test_norm, y_test_cat, verbose=0)
cnn_params = cnn_model.count_params()

print(f"Accuracy du CNN sur le test set : {cnn_acc:.4f} ({cnn_acc*100:.2f}%)")
print(f"Nombre de paramètres du CNN : {cnn_params:,}")
#Accuracy du CNN sur le test set : 0.3850 (38.50%)
#Nombre de paramètres du CNN : 643,492
```
![[Pasted image 20261026100836.png]]


## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `filters` | cartes produites |
| `kernel_size` | taille du filtre |
| `strides` | pas |
| `padding` | same ou valid |
**Padding `"same"`** : on ajoute des zéros autour de l'image pour que la sortie ait la **même taille** que l'entrée.

**Padding `"valid"`** : pas d'ajout de zéros, la sortie est **plus petite** que l'entrée.

**Stride** : le nombre de pixels dont le filtre se déplace à chaque pas. Un stride de 2 **divise par 2** la taille spatiale.

La taille de sortie se calcule avec la formule (pour chaque dimension) :
![[Pasted image 20261026101909.png]]


## Points de vigilance

- La forme usuelle est batch, hauteur, largeur, canaux.
 **Erreur fréquente :** Utiliser `padding="valid"` (le défaut de Keras) réduit la taille spatiale à chaque convolution. Sur de petites images comme CIFAR (32x32), on perd rapidement trop d'information. On préfèrera `padding="same"` pour contrôler la réduction via le pooling uniquement.
## Documentation officielle

- [Référence](https://keras.io/api/layers/convolution_layers/convolution2d/)

![[Pasted image 20261026101537.png]]

![[Pasted image 20261026101734.png]]

```python
import numpy as np
import matplotlib.pyplot as plt
from tensorflow import keras
from tensorflow.keras import layers, models, callbacks, regularizers
from tensorflow.keras.datasets import cifar100
import time
np.random.seed(42)
keras.utils.set_random_seed(42)

(X_train, y_train), (X_test, y_test) = cifar100.load_data()

# Normalisation
X_train_norm = X_train.astype("float32") / 255.0
X_test_norm = X_test.astype("float32") / 255.0

# One-hot encoding
num_classes = 100
y_train_cat = keras.utils.to_categorical(y_train, num_classes)
y_test_cat = keras.utils.to_categorical(y_test, num_classes)

print(f"X_train : {X_train_norm.shape}")
print(f"X_test  : {X_test_norm.shape}")
print(f"Nombre de classes : {num_classes}")

# mini-pipeline d'augmentation
data_augmentation = models.Sequential([
    layers.RandomFlip("horizontal"),
    layers.RandomRotation(0.1),
    layers.RandomZoom(0.1),
    layers.RandomTranslation(0.1, 0.1),
], name="data_augmentation")

# 1 image originale vs 8 versions augmentées
sample_img = X_train_norm[0:1]  # Shape (1, 32, 32, 3)

fig, axes = plt.subplots(1, 9, figsize=(18, 3))
axes[0].imshow(sample_img[0])
axes[0].set_title("Original", fontsize=10, fontweight="bold")
axes[0].axis("off")

for i in range(1, 9):
    augmented = data_augmentation(sample_img, training=True)
    axes[i].imshow(augmented[0].numpy())
    axes[i].set_title(f"Aug. {i}", fontsize=10)
    axes[i].axis("off")

plt.tight_layout()
plt.show()

# fonction pour afficher les courbes d'apprentissage
def plot_history(history, title=""):
    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(14, 5))
    
    ax1.plot(history.history["accuracy"], label="Train", linewidth=2)
    ax1.plot(history.history["val_accuracy"], label="Validation", linewidth=2)
    ax1.set_title(f"{title} — Accuracy")
    ax1.set_xlabel("Epoch")
    ax1.set_ylabel("Accuracy")
    ax1.legend()
    ax1.grid(True, alpha=0.3)
    
    ax2.plot(history.history["loss"], label="Train", linewidth=2)
    ax2.plot(history.history["val_loss"], label="Validation", linewidth=2)
    ax2.set_title(f"{title} — Loss")
    ax2.set_xlabel("Epoch")
    ax2.set_ylabel("Loss")
    ax2.legend()
    ax2.grid(True, alpha=0.3)
    
    plt.tight_layout()
    plt.show()

model = models.Sequential([
    # augmentation des données
    layers.RandomFlip("horizontal", input_shape=(32, 32, 3)),
    layers.RandomRotation(0.1),
    layers.RandomZoom(0.1),
    layers.RandomTranslation(0.1, 0.1),
    
    # Bloc 1
    layers.Conv2D(32, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.Conv2D(32, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),
    layers.Dropout(0.25),
    
    # Bloc 2
    layers.Conv2D(64, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.Conv2D(64, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),
    layers.Dropout(0.25),
    
    # Bloc 3
    layers.Conv2D(128, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.Conv2D(128, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),
    layers.Dropout(0.25),
    
    # Classification
    layers.Flatten(),
    layers.Dense(256, activation="relu"),
    layers.BatchNormalization(),
    layers.Dropout(0.5),
    layers.Dense(num_classes, activation="softmax")
], name="model_complet")

model.compile(optimizer="adam", loss="categorical_crossentropy", metrics=["accuracy"])
model.summary()


#Définir un earlystop
early_stop = callbacks.EarlyStopping(
    monitor="val_loss",
    patience=7,
    restore_best_weights=True,
    min_delta=0.001,
    verbose=1
)

start_c = time.time()
history = model.fit(
    X_train_norm, y_train_cat,
    epochs=80,
    batch_size=64,
    validation_split=0.2,
    callbacks=[early_stop],
    verbose=1
)
time_c = time.time() - start_c
print(f"\nTemps d'entraînement modèle complet : {time_c:.1f}s")
print(f"Nombre d'époques effectives : {len(history.history['loss'])}")


plot_history(history, "Modèle Complet")

_, acc = model.evaluate(X_test_norm, y_test_cat, verbose=0)

print(acc)

```