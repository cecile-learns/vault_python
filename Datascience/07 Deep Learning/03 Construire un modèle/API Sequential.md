# Sequential

## Quand l'utiliser ?

Empiler des couches dans un flux simple.

## Pattern

```python
import tensorflow as tf
model=tf.keras.Sequential([
 tf.keras.Input(shape=(20,)),
 tf.keras.layers.Dense(64,activation="relu"),
 tf.keras.layers.Dense(1,activation="sigmoid")])
 
 #cours
 from tensorflow.keras import Sequential, layers

model_seq = Sequential([
    layers.Input(shape=(4,)),
    layers.Dense(64, activation='relu'),
    layers.Dense(num_classes, activation='softmax')
])
 
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `layers` | couches ordonnées |
| `name` | nom facultatif |

## Points de vigilance

- Passer à l'API fonctionnelle si le graphe se ramifie.

## Documentation officielle

- [Référence](https://keras.io/api/models/sequential/)


### API Séquentielle

Il s'agit de la plus simple : elle permet de construire un réseau linéaire de couches, c'est-à-dire **un seul flux d’entrée et un seul flux de sortie avec pas de branchements**. Par exemple, le code suivant permet de créer un réseau de neurones à 2 couches : la première avec 64 neurones et une activation avec ReLU, et la seconde avec 10 neurones et une activation avec softmax :

```python
from tensorflow.keras import Sequential, layers

model = Sequential([
   layers.Dense(64, activation='relu'),
   layers.Dense(10, activation='softmax')
])
```

Cette écriture est très **simple et rapide** à utiliser, est lisible et idéale pour les architectures empilées classiques (MLP, CNN, etc.). Son seul défaut est qu'elle ne permet pas de créer des graphiques non linéaires, c'est-à-dire avec multi-input / multi-output, ou des branches parallèles.