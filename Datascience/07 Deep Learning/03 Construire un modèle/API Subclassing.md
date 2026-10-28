



Il s'agit d'une API orientée objet : le modèle est défini en sous-classant `tf.keras.Model` et en réimplémentant la méthode `call()`. Cela permet un contrôle total sur les opérations effectuées et est idéal pour les architectures non standard, mais vient au prix d'un code plus lourd. A titre informatif, le même modèle peut être implémenté avec :

```python
import tensorflow as tf
from tensorflow.keras import layers

class MyCustomModel(tf.keras.Model):
   def __init__(self):
       super().__init__()
       self.d1 = layers.Dense(64, activation='relu')
       self.d2 = layers.Dense(10, activation='softmax')

   def call(self, inputs):
       x = self.d1(inputs)
       return self.d2(x)

model = MyCustomModel()
```

La programation orientée objet est un domaine très populaire dans de nombreux langages de programmation. N'hésitez pas à vous référer au module `140 - Programmation orientée` objet pour plus d'informations.