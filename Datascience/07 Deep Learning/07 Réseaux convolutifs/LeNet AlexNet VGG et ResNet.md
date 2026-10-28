# LeNet AlexNet VGG et ResNet

## Quand l'utiliser ?

Repérer la logique des architectures CNN rencontrées dans le cours.

## Pattern

### AlexNet (2012)

```python
import numpy as np
import matplotlib.pyplot as plt
from tensorflow import keras
from tensorflow.keras import layers, models, callbacks
from tensorflow.keras.datasets import cifar100
import time

np.random.seed(42)
keras.utils.set_random_seed(42)

(X_train, y_train), (X_test, y_test) = cifar100.load_data()
X_train_norm = X_train.astype("float32") / 255.0
X_test_norm = X_test.astype("float32") / 255.0
num_classes = 100
y_train_cat = keras.utils.to_categorical(y_train, num_classes)
y_test_cat = keras.utils.to_categorical(y_test, num_classes)

class_names_fm = ["T-shirt", "Pantalon", "Pull", "Robe", "Manteau",
                   "Sandale", "Chemise", "Sneaker", "Sac", "Bottine"]

alexnet = models.Sequential([
    # Couche 1 : filtre réduit de 11x11 à 3x3
    layers.Conv2D(96, (3, 3), activation="relu", padding="same", input_shape=(32, 32, 3)),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),

    # Couche 2 : filtre réduit de 5x5 à 3x3
    layers.Conv2D(256, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),

    # Couches 3, 4, 5 : filtres 3x3 (identiques à l'original)
    layers.Conv2D(384, (3, 3), activation="relu", padding="same"),
    layers.Conv2D(384, (3, 3), activation="relu", padding="same"),
    layers.Conv2D(256, (3, 3), activation="relu", padding="same"),
    layers.MaxPooling2D((2, 2)),

    # Classification (FC layers réduits)
    layers.Flatten(),
    layers.Dense(1024, activation="relu"),
    layers.Dropout(0.5),
    layers.Dense(1024, activation="relu"),
    layers.Dropout(0.5),
    layers.Dense(num_classes, activation="softmax")
], name="AlexNet")

alexnet.compile(optimizer="adam", loss="categorical_crossentropy", metrics=["accuracy"])
alexnet.summary()


```
![[Pasted image 20261026104517.png]]

Ses innovations clés :
- Utilisation de **ReLU** au lieu de tanh/sigmoid (convergence 6x plus rapide)
- Entraînement sur **GPU** (2 GPU en parallèle)
- **Dropout** pour la régularisation (inventé dans ce contexte)
- **Data Augmentation** (crops, flips, color jittering)
- Filtres de tailles variées : **11x11**, **5x5**, **3x3**
L'architecture originale prenait des images de **227x227x3**, mais nous l'**adaptons à CIFAR-100** (32x32x3) en réduisant les tailles de filtres et en supprimant certains pooling.


### VGGNet (2014)
**Définition :** **VGGNet** a prouvé que la **profondeur** du réseau (nombre de couches) est un facteur déterminant de la performance, à condition d'utiliser des filtres **petits et uniformes** (3x3). Ce principe a guidé toutes les architectures suivantes.
```python
# blocs de Conv3x3 empilées + MaxPool, avec doublement des filtres
vgg_like = models.Sequential([
    # Bloc 1 : 2 x Conv 64
    layers.Conv2D(64, (3, 3), activation="relu", padding="same", input_shape=(32, 32, 3)),
    layers.BatchNormalization(),
    layers.Conv2D(64, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),
    layers.Dropout(0.25),

    # Bloc 2 : 2 x Conv 128
    layers.Conv2D(128, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.Conv2D(128, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),
    layers.Dropout(0.25),

    # Bloc 3 : 3 x Conv 256
    layers.Conv2D(256, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.Conv2D(256, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.Conv2D(256, (3, 3), activation="relu", padding="same"),
    layers.BatchNormalization(),
    layers.MaxPooling2D((2, 2)),
    layers.Dropout(0.25),

    # Classification
    layers.Flatten(),
    layers.Dense(512, activation="relu"),
    layers.BatchNormalization(),
    layers.Dropout(0.5),
    layers.Dense(num_classes, activation="softmax")
], name="VGG_like")

vgg_like.compile(optimizer="adam", loss="categorical_crossentropy", metrics=["accuracy"])
vgg_like.summary()

```

![[Pasted image 20261026104711.png]]
  **Erreur fréquente :** VGG-16 original utilise **3 couches denses** de 4096-4096-1000 neurones, ce qui représente **~124M des 138M paramètres totaux** ! C'est un gaspillage énorme. Les architectures modernes utilisent un `GlobalAveragePooling2D` pour éviter cette explosion de paramètres.

### ResNet (2015)
**Définition :** Un **bloc résiduel** (_residual block_) ajoute l'entrée **directement** à la sortie du bloc : `output = F(x) + x`. Si le réseau n'a rien d'utile à apprendre dans ce bloc, il peut simplement mettre F(x) ≈ 0 et propager l'identité. Cela permet de construire des réseaux de **centaines de couches** sans dégradation.
```python
def residual_block(x, filters, downsample=False):
    """
    Bloc résiduel standard.
    Si downsample=True, la première conv utilise stride=2
    et un shortcut avec Conv 1x1 est ajouté.
    """
    stride = 2 if downsample else 1
    shortcut = x

    # Branche principale
    x = layers.Conv2D(filters, (3, 3), strides=stride, padding="same", use_bias=False)(x)
    x = layers.BatchNormalization()(x)
    x = layers.Activation("relu")(x)

    x = layers.Conv2D(filters, (3, 3), strides=1, padding="same", use_bias=False)(x)
    x = layers.BatchNormalization()(x)

    # Adapter le shortcut si les dimensions changent
    if downsample or shortcut.shape[-1] != filters:
        shortcut = layers.Conv2D(filters, (1, 1), strides=stride, padding="same", use_bias=False)(shortcut)
        shortcut = layers.BatchNormalization()(shortcut)

    # Addition + activation
    x = layers.Add()([x, shortcut])
    x = layers.Activation("relu")(x)
    return x

inputs = layers.Input(shape=(32, 32, 3))

# Couche initiale
x = layers.Conv2D(64, (3, 3), padding="same", use_bias=False)(inputs)
x = layers.BatchNormalization()(x)
x = layers.Activation("relu")(x)

# Blocs résiduels
# Bloc 1 : 64 filtres, pas de downsampling
x = residual_block(x, 64)
x = residual_block(x, 64)

# Bloc 2 : 128 filtres, downsampling (32->16)
x = residual_block(x, 128, downsample=True)
x = residual_block(x, 128)

# Bloc 3 : 256 filtres, downsampling (16->8)
x = residual_block(x, 256, downsample=True)
x = residual_block(x, 256)

# Classification
x = layers.GlobalAveragePooling2D()(x)
x = layers.Dropout(0.5)(x)
outputs = layers.Dense(num_classes, activation="softmax")(x)

resnet = models.Model(inputs, outputs, name="ResNet")
resnet.compile(optimizer="adam", loss="categorical_crossentropy", metrics=["accuracy"])
resnet.summary()


```






![[Pasted image 20261026105059.png]]






