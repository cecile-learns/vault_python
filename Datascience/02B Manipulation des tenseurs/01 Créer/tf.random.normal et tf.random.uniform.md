# tf.random.normal et tf.random.uniform

## Syntaxe

```python
import tensorflow as tf

t_normal = tf.random.normal(
    shape=(3, 4),
    mean=0.0,
    stddev=1.0,
    dtype=tf.float32,
    seed=42,
)

t_uniforme = tf.random.uniform(
    shape=(3, 4),
    minval=0.0,
    maxval=1.0,
    dtype=tf.float32,
    seed=42,
)
```

## Paramètres

| Paramètre | Rôle |
|---|---|
| `shape` | dimensions de sortie |
| `mean`, `stddev` | moyenne et écart-type de la loi normale |
| `minval`, `maxval` | bornes de la loi uniforme ; borne haute exclue |
| `dtype` | type des valeurs |
| `seed` | reproductibilité du tirage |

## Fonction

Créer des tenseurs de valeurs aléatoires selon une loi normale ou uniforme.
