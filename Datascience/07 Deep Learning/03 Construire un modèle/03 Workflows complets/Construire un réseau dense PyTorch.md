# Construire un réseau dense PyTorch

## Pour faire quoi ?

Créer un classifieur dense adapté automatiquement aux dimensions des données.

## Script réutilisable

```python
import torch.nn as nn

nombre_features = X_train.shape[1]
nombre_classes = len(classes)

model = nn.Sequential(
    nn.Linear(nombre_features, 100),
    nn.ReLU(),
    nn.Linear(100, 50),
    nn.ReLU(),
    nn.Linear(50, nombre_classes),
)
```

## À adapter

- nombre et largeur des couches ;
- activation ;
- dimension de sortie ;
- ajout de Dropout si l'overfitting le justifie.

## Contrôles

```python
x_batch, _ = next(iter(train_loader))
logits = model(x_batch)
print(x_batch.shape, logits.shape)
```

La seconde dimension de `logits` doit correspondre au nombre de classes.
