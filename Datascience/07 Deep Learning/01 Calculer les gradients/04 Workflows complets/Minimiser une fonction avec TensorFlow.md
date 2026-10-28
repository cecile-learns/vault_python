# Minimiser une fonction avec TensorFlow

## Pour faire quoi ?

Calculer automatiquement le gradient d'une fonction puis modifier une variable avec un optimiseur.

## Script réutilisable

```python
import tensorflow as tf

x = tf.Variable(2.0)
optimizer = tf.keras.optimizers.Adam(learning_rate=0.1)

for iteration in range(100):
    with tf.GradientTape() as tape:
        valeur = x**2 - x + 2

    gradient = tape.gradient(valeur, x)
    optimizer.apply_gradients([(gradient, x)])

print("x* :", x.numpy())
print("f(x*) :", valeur.numpy())
```

## Paramètres et hyperparamètres

| Élément | Rôle | Réglage utile |
|---|---|---|
| `learning_rate` | amplitude des mises à jour | diminuer si la valeur oscille ou diverge |
| `range(100)` | nombre d'itérations | augmenter si la convergence est lente |
| `Adam` | règle de mise à jour | remplacer par `SGD` pour une descente plus directe |

## À adapter

- variable optimisée ;
- fonction calculée dans `GradientTape` ;
- learning rate et nombre d'itérations.

## Vigilances & tips

Le calcul de la fonction doit avoir lieu dans le bloc `GradientTape`.

## Commandes utilisées

`tf.Variable`, `tf.GradientTape`, `tape.gradient`, `Adam`, `apply_gradients`.
