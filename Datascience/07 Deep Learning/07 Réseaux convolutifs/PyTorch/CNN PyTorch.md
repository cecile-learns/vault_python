# CNN PyTorch

## Pattern

```python
import torch.nn as nn

model = nn.Sequential(
    nn.Conv2d(3, 16, kernel_size=3, padding=1),
    nn.ReLU(),
    nn.MaxPool2d(kernel_size=2),
    nn.Conv2d(16, 32, kernel_size=3, padding=1),
    nn.ReLU(),
    nn.MaxPool2d(kernel_size=2),
    nn.Flatten(),
    nn.Linear(32 * 56 * 56, nombre_classes),
)
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `in_channels` | canaux reçus | 3 en RGB, 1 en niveaux de gris |
| `out_channels` | filtres produits | augmente généralement au fil des blocs |
| `kernel_size` | taille du filtre | 3 dans l'exemple du cours |
| `stride` | pas du filtre | supérieur à 1 réduit la taille spatiale |
| `padding` | bord ajouté | `1` conserve la taille avec un noyau 3 |
| `MaxPool2d` | sous-échantillonnage | noyau 2 divise hauteur et largeur par 2 |

## Vigilances & tips

Calculer la forme après les convolutions avant de fixer `in_features` de la couche linéaire.
