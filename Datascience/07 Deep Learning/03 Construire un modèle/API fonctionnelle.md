# API fonctionnelle

## Quand l'utiliser ?

Créer plusieurs entrées, sorties ou connexions.

## Pattern

```python
import tensorflow as tf
inputs=tf.keras.Input(shape=(20,),name="features")
x=tf.keras.layers.Dense(64,activation="relu")(inputs)
outputs=tf.keras.layers.Dense(3,activation="softmax")(x)
model=tf.keras.Model(inputs=inputs,outputs=outputs)


# Cours : implémenter  avec l'API Fonctionnelle de Keras, l'architecture d'un réseau de neurones à 5 couches, respectivement de 64, 64, 32, 16, et 1 neurone.

from tensorflow.keras import Model, layers, Input

inputs = Input(shape=(X_train.shape[1],))# Définit la forme des données qui entre dans le réseau. plus adapte que mettre les chiffres shape=(4,)

x = layers.Dense(64, activation='relu')(inputs)
x = layers.Dense(64, activation='relu')(x)
x = layers.Dense(32, activation='relu')(x)
x = layers.Dense(16, activation='relu')(x)
x = layers.Dense(1, activation='relu')(x)

outputs = layers.Dense(1, activation='linear')(x)

model = Model(inputs, outputs)

model.summary()
```
![[Pasted image 20261024121256.png]]
## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `inputs` | entrée |
| `outputs` | sortie |

## Points de vigilance

- Nommer les sorties des modèles multi-tâches.

## Documentation officielle

- [Référence](https://keras.io/api/models/model/)

### API Fonctionnelle

Pour tout modèle avec topologies complexes, on préférera cette API : c'est la **plus utilisée en pratique** pour les architectures industrielles. Elle permet de représenter explicitement les graphes computationnels, les relations entre couches, et les **topologies complexes** (multi-input, etc.). Voici le même réseau de neurones implémenté avec cette API :

```python
from tensorflow.keras import Model, layers, Input

inputs = Input(shape=(32,))
x = layers.Dense(64, activation='relu')(inputs)
outputs = layers.Dense(10, activation='softmax')(x)

model = Model(inputs, outputs)
```

Ici, l'input est un tenseur de 32 dimensions : `Input(shape=(32,))` permet de créer un nœud d'entrée dans le graphe computationnel : ce n'est pas une valeur explicite, mais un _placeholder_. `layers.Dense(...)(inputs)` applique la couche à l’objet `inputs` → ceci crée un nœud intermédiaire dans le graphe. `Model(inputs, outputs)` définit le modèle comme un graphe reliant explicitement entrée et sortie. Ici, chaque opération construit un DAG (Directed Acyclic Graph) : c'est une architecture déclarative.

En comparaison, le code de **l'API Séquentielle empile strictement linéairement les couches**, et l'entrée est déduite automatiquement lors de l'entraînement du modèle.