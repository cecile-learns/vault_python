# Sampling et loss VAE

## Syntaxe

```python
import tensorflow as tf
from tensorflow.keras import layers

class Sampling(layers.Layer):
    def call(self, inputs):
        z_mean, z_log_var = inputs
        epsilon = tf.keras.backend.random_normal(shape=tf.shape(z_mean))
        return z_mean + tf.exp(0.5 * z_log_var) * epsilon

z = Sampling()([z_mean, z_log_var])

reconstruction_loss = tf.reduce_mean(
    tf.reduce_sum(
        tf.keras.losses.binary_crossentropy(entree, reconstruction),
        axis=1,
    )
)

kl_loss = -0.5 * tf.reduce_mean(
    tf.reduce_sum(
        1 + z_log_var - tf.square(z_mean) - tf.exp(z_log_var),
        axis=1,
    )
)

loss_totale = reconstruction_loss + kl_loss
```

## Paramètres et hyperparamètres

| Élément | Rôle | Réglage utile |
|---|---|---|
| dimension de `z_mean` | dimension de l'espace latent | 2 pour visualiser, davantage pour une représentation plus riche |
| reconstruction loss | fidélité de la reconstruction | cohérente avec la normalisation et le type de données |
| KL loss | rapproche l'espace latent d'une loi normale | peut être pondérée pour modifier le compromis |
| `epsilon` | bruit aléatoire de l'échantillonnage | généré à chaque passage |

## Vigilances & tips

La loss d'un VAE n'est pas seulement une loss de reconstruction : oublier la divergence KL supprime la régularisation de l'espace latent.
