# EarlyStopping

## Quand l'utiliser ?

Arrêter quand la validation ne s'améliore plus.

## Pattern

```python
import tensorflow as tf
callback=tf.keras.callbacks.EarlyStopping(monitor="val_loss",patience=5,restore_best_weights=True)



tf.keras.callbacks.EarlyStopping(
       monitor="val_loss",
       patience=20,
       restore_best_weights=True
   )
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `monitor` | mesure |
| `patience` | epochs tolérées |
| `restore_best_weights` | récupère les meilleurs poids |

## Points de vigilance

- Fournir une validation si monitor commence par val_.

## Documentation officielle

- [Référence](https://keras.io/api/callbacks/early_stopping/)


avec cnn ; 
![[Pasted image 20261026102840.png]]