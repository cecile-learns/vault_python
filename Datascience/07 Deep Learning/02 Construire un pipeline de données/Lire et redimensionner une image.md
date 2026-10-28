# Lire et redimensionner une image

## Quand l'utiliser ?

Décoder une image puis uniformiser sa taille.

## Pattern

```python
import tensorflow as tf
def charger_image(path):
    contenu=tf.io.read_file(path)
    image=tf.image.decode_jpeg(contenu,channels=3)
    image=tf.image.resize(image,(224,224))
    return tf.cast(image,tf.float32)/255.0
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `channels` | nombre de canaux |
| `size` | hauteur et largeur |
| `method` | interpolation |

## Points de vigilance

- Adapter le décodeur au format.
- Respecter le prétraitement du modèle.

## Documentation officielle

- [Référence](https://www.tensorflow.org/api_docs/python/tf/image)
