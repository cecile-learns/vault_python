# ModelCheckpoint

## Quand l'utiliser ?

Sauvegarder le meilleur modèle.

## Pattern

```python
import tensorflow as tf
callback=tf.keras.callbacks.ModelCheckpoint("meilleur.keras",monitor="val_loss",save_best_only=True)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `filepath` | fichier |
| `save_best_only` | garde les améliorations |
| `save_weights_only` | poids seuls |

## Points de vigilance

- Utiliser l'extension adaptée.

## Documentation officielle

- [Référence](https://keras.io/api/callbacks/model_checkpoint/)
