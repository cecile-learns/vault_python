# Couche personnalisée

## Quand l'utiliser ?

Encapsuler des poids et un calcul réutilisable.

## Pattern

```python
import tensorflow as tf
class CoucheLineaire(tf.keras.layers.Layer):
    def build(self,input_shape):
        self.w=self.add_weight(shape=(input_shape[-1],1),initializer="random_normal",trainable=True)
        self.b=self.add_weight(shape=(1,),initializer="zeros",trainable=True)
    def call(self,inputs):
        return tf.matmul(inputs,self.w)+self.b
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `build` | crée les poids |
| `call` | décrit le calcul |

## Points de vigilance

- Créer les poids avec add_weight.

## Documentation officielle

- [Référence](https://keras.io/api/layers/base_layer/)
