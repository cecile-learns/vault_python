# compile

## Quand l'utiliser ?

Choisir optimiseur, perte et métriques.

## Pattern

```python
import tensorflow as tf
model=tf.keras.Sequential([tf.keras.Input((10,)),tf.keras.layers.Dense(1,activation="sigmoid")])
model.compile(optimizer="adam",loss="binary_crossentropy",metrics=["accuracy"])


#cours
from tensorflow.keras.optimizers import SGD

model_seq.compile(
    optimizer=SGD(learning_rate=1e-3),
    loss='categorical_crossentropy',
    metrics=['accuracy']
)


```

categorical_crossentropy, binary_crossentropy, mse


```python
#Optimizer Adam
from tensorflow.keras.optimizers import Adam

model.compile(
   optimizer=tf.keras.optimizers.Adam(learning_rate=1e-3),
   # Le LR est fixé pour le début, et adapté par la suite
   loss="mse",
   metrics=["mae"]
)
```
## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `optimizer` | mise à jour |
| `loss` | objectif optimisé |
| `metrics` | indicateurs |
# Fit
## Quand l'utiliser ?

Entraîner et conserver l'historique.

## Pattern

```python
import tensorflow as tf
model=tf.keras.Sequential([tf.keras.Input((4,)),tf.keras.layers.Dense(1)])
model.compile(optimizer="adam",loss="mse")
x=tf.ones((40,4)); y=tf.ones((40,1))
history=model.fit(x,y,epochs=10,batch_size=8,validation_split=0.2,verbose=1)

#cours
history = model_seq.fit(
    X_train, y_train,
    batch_size=16,
    epochs=50,
    verbose=1
)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `epochs` | passes |
| `batch_size` | taille des lots |
| `validation_data` | validation |
| `callbacks` | actions |

## Points de vigilance

- Le test reste séparé de la validation.

- Une métrique n'est pas nécessairement optimisée.

## Documentation officielle

- [Référence](https://keras.io/api/models/model_training_apis/)
