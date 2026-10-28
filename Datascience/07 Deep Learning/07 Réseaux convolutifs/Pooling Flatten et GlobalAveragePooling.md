# Pooling Flatten et GlobalAveragePooling

## Quand l'utiliser ?

Réduire l'espace puis préparer la sortie.

## Pattern

```python
import tensorflow as tf
max_pool=tf.keras.layers.MaxPooling2D(pool_size=(2,2))
flatten=tf.keras.layers.Flatten()
gap=tf.keras.layers.GlobalAveragePooling2D() #same : tf.keras.layers.MaxPooling2D(pool_size=(2, 2))

#Version flatten
model = keras.Sequential([
    layers.Input(shape=(32, 32, 3)),
    layers.Conv2D(32, (3, 3), activation="relu", padding="same"),
    layers.MaxPooling2D((2, 2)),
    layers.Flatten(),
    layers.Dense(100, activation="softmax")
])

#Version globalAveragePooling2D
model = keras.Sequential([
    layers.Input(shape=(32, 32, 3)),
    layers.Conv2D(32, (3, 3), activation="relu", padding="same"),
    layers.MaxPooling2D((2, 2)),
    layers.GlobalAveragePooling2D(),
    layers.Dense(100, activation="softmax")
])
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `pool_size` | fenêtre |
| `strides` | pas |
| `GlobalAveragePooling2D` | une moyenne par canal |
- `MaxPooling2D` : réduit la hauteur et la largeur des cartes de caractéristiques, en conservant le nombre de canaux.
    
- `Flatten` : transforme toutes les cartes en un long vecteur, sans calculer de moyenne.
    
- `GlobalAveragePooling2D` : calcule une moyenne par carte, produisant un vecteur beaucoup plus court.
## Points de vigilance

- Flatten peut créer beaucoup de paramètres.

## Documentation officielle

- [Référence](https://keras.io/api/layers/pooling_layers/)
