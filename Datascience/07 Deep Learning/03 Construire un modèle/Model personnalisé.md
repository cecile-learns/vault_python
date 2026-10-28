# Model personnalisé

## Quand l'utiliser ?

Définir un modèle réutilisable par sous-classement.

## Pattern

```python
import tensorflow as tf

class MLP(tf.keras.Model):
    def __init__(self):
        super().__init__()
        self.hidden=tf.keras.layers.Dense(32,activation="relu")
        self.output_layer=tf.keras.layers.Dense(1,activation="sigmoid")

    def call(self,inputs,training=False):
        return self.output_layer(self.hidden(inputs))
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `__init__` | déclare les couches |
| `call` | décrit le passage avant |

## Points de vigilance

- Déclarer les couches dans __init__.

## Documentation officielle

- [Référence](https://keras.io/api/models/model/)
