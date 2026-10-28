# evaluate predict et history

## Quand l'utiliser ?

Évaluer, produire des sorties et diagnostiquer les courbes.

## Pattern

```python
import tensorflow as tf
model=tf.keras.Sequential([tf.keras.Input((4,)),tf.keras.layers.Dense(1)])
model.compile(optimizer="adam",loss="mse")
x=tf.ones((8,4)); y=tf.ones((8,1))
resultats=model.evaluate(x,y,verbose=0)
predictions=model.predict(x,verbose=0)
model.summary()
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `return_dict` | résultats nommés |
| `verbose` | affichage |
| `history.history` | séries par epoch |

## Points de vigilance

- predict donne la sortie, pas toujours une classe.

## Documentation officielle

- [Référence](https://keras.io/api/models/model_training_apis/)
