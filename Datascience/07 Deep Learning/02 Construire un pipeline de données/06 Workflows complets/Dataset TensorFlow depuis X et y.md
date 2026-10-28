# Dataset TensorFlow depuis X et y

## Pour faire quoi ?

Créer des pipelines train et test à partir de tableaux déjà préparés.

## Script réutilisable

```python
import tensorflow as tf

train_dataset = tf.data.Dataset.from_tensor_slices((X_train, y_train))
train_dataset = train_dataset.shuffle(
    buffer_size=len(X_train),
    seed=42,
    reshuffle_each_iteration=True,
)
train_dataset = train_dataset.batch(32)
train_dataset = train_dataset.prefetch(tf.data.AUTOTUNE)

test_dataset = tf.data.Dataset.from_tensor_slices((X_test, y_test))
test_dataset = test_dataset.batch(32).prefetch(tf.data.AUTOTUNE)
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `buffer_size` | réserve utilisée pour le mélange | toute la taille si elle tient en mémoire |
| `batch_size` | observations par mise à jour | tester 16, 32 ou 64 |
| `seed` | reproductibilité | conserver pour comparer les essais |
| `reshuffle_each_iteration` | remélange à chaque epoch | généralement `True` pour le train |
| `AUTOTUNE` | règle automatiquement le préchargement | choix par défaut utile |

## Vigilances & tips

- Ne pas mélanger le test.
- `X` et `y` doivent avoir la même première dimension.
- Ajuster les transformations apprises uniquement sur le train.
