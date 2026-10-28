# Boucle d'entraînement TensorFlow

## Pour faire quoi ?

Contrôler explicitement le calcul de la loss et la mise à jour des poids sans utiliser `model.fit()`.

## Script réutilisable

```python
import tensorflow as tf

optimizer = tf.keras.optimizers.Adam(learning_rate=1e-3)
loss_fn = tf.keras.losses.SparseCategoricalCrossentropy()
metric_train = tf.keras.metrics.SparseCategoricalAccuracy()

@tf.function
def train_step(x_batch, y_batch):
    with tf.GradientTape() as tape:
        predictions = model(x_batch, training=True)
        loss = loss_fn(y_batch, predictions)

    gradients = tape.gradient(loss, model.trainable_variables)
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))
    metric_train.update_state(y_batch, predictions)
    return loss

for epoch in range(10):
    metric_train.reset_state()
    for x_batch, y_batch in train_dataset:
        loss = train_step(x_batch, y_batch)

    print(
        f"Epoch {epoch + 1} — loss={float(loss):.4f} — "
        f"accuracy={float(metric_train.result()):.4f}"
    )
```

## Paramètres et hyperparamètres

| Élément | Rôle | Réglage utile |
|---|---|---|
| `learning_rate` | vitesse de mise à jour | commencer autour de `1e-3` avec Adam puis ajuster |
| `from_logits` | signale des logits en sortie | `True` sans softmax final, sinon `False` |
| `training=True` | active le mode entraînement | nécessaire pour Dropout et BatchNormalization |
| nombre d'epochs | passages sur le train | régler à partir des performances de validation |

## À adapter

- loss et métrique selon la cible ;
- modèle et Dataset ;
- learning rate et nombre d'epochs.

## Vigilances & tips

- Tester d'abord sans `@tf.function` pour faciliter le débogage.
- Utiliser `training=False` dans la boucle d'évaluation.
- Contrôler les gradients `None` avant leur application.
