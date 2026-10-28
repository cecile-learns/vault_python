# ReduceLROnPlateau

## Quand l'utiliser ?

Réduire le taux lorsque la progression stagne.

## Pattern

```python
import tensorflow as tf
callback=tf.keras.callbacks.ReduceLROnPlateau(monitor="val_loss",factor=0.2,patience=3,min_lr=1e-6)



# Si la val_loss ne s'améliore pas après 10 epochs, LR = LR * 0.5
lr_scheduler = ReduceLROnPlateau(
   monitor='val_loss',
   factor=0.5,
   patience=10,
   verbose=1,
   min_lr=1e-6
)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `factor` | multiplicateur |
| `min_lr` | borne inférieure |

## Points de vigilance

- Ne pas réduire trop vite sur une mesure bruitée.

## Documentation officielle

- [Référence](https://keras.io/api/callbacks/reduce_lr_on_plateau/)
