# Pipeline TensorFlow depuis des chemins d'images

## Pour faire quoi ?

Lire, décoder, redimensionner et regrouper des images dont les chemins et labels sont connus.

## Script réutilisable

```python
import tensorflow as tf

HAUTEUR = 224
LARGEUR = 224
BATCH_SIZE = 32

def charger_image(chemin, label):
    contenu = tf.io.read_file(chemin)
    image = tf.image.decode_jpeg(contenu, channels=3)
    image = tf.image.resize(image, (HAUTEUR, LARGEUR))
    image = tf.cast(image, tf.float32) / 255.0
    return image, label

train_dataset = tf.data.Dataset.from_tensor_slices((chemins_train, labels_train))
train_dataset = train_dataset.shuffle(len(chemins_train), seed=42)
train_dataset = train_dataset.map(
    charger_image,
    num_parallel_calls=tf.data.AUTOTUNE,
)
train_dataset = train_dataset.batch(BATCH_SIZE).prefetch(tf.data.AUTOTUNE)
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `channels` | nombre de canaux | `3` pour RGB, `1` pour niveaux de gris |
| hauteur et largeur | forme attendue par le modèle | reprendre l'input shape ou le backbone |
| `BATCH_SIZE` | images par lot | réduire si la mémoire est insuffisante |
| `num_parallel_calls` | lectures simultanées | `AUTOTUNE` dans la majorité des cas |

## À adapter

- utiliser `decode_png` pour des PNG ;
- remplacer `/ 255.0` par le `preprocess_input` du modèle préentraîné si nécessaire ;
- appliquer l'augmentation uniquement au train.
