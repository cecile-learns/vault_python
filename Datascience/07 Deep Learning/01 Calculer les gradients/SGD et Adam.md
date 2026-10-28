# SGD et Adam

## Quand l'utiliser ?

Mettre à jour les poids à partir des gradients.

## Pattern

```python
import tensorflow as tf
sgd=tf.keras.optimizers.SGD(learning_rate=0.01,momentum=0.9)
adam=tf.keras.optimizers.Adam(learning_rate=0.001)



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
| `learning_rate` | amplitude des mises à jour |
| `momentum` | inertie de SGD |
| `clipnorm` | limite la norme |

## Points de vigilance

- Un taux trop grand diverge; trop petit ralentit.

## Documentation officielle

- [Référence](https://keras.io/api/optimizers/)
