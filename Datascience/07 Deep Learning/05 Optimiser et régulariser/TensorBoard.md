# TensorBoard

## Quand l'utiliser ?

Enregistrer les métriques et informations d’expérience.

## Pattern

```python
import tensorflow as tf

callback=tf.keras.callbacks.TensorBoard(log_dir="logs/experience_01",histogram_freq=1)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `log_dir` | dossier des journaux |
| `histogram_freq` | fréquence des histogrammes |

## Points de vigilance

- Séparer le dossier de chaque expérience.

## Documentation officielle

- [Référence](https://keras.io/api/callbacks/tensorboard/)
