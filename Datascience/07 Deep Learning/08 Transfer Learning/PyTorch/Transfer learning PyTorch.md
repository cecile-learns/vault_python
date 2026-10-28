# Transfer learning PyTorch

## Script réutilisable

```python
import torch
import torch.nn as nn
from torchvision import models
from torchvision.models import ResNet50_Weights

weights = ResNet50_Weights.DEFAULT
model = models.resnet50(weights=weights)

for parametre in model.parameters():
    parametre.requires_grad = False

nombre_features = model.fc.in_features
model.fc = nn.Linear(nombre_features, nombre_classes)

optimizer = torch.optim.Adam(model.fc.parameters(), lr=1e-3)
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `weights` | poids préentraînés | `DEFAULT` sélectionne les poids recommandés |
| `requires_grad=False` | gèle le backbone | dégeler ensuite certaines couches pour fine-tuner |
| `nombre_classes` | sorties de la tête | nombre de classes du problème |
| learning rate | vitesse d'apprentissage | plus faible lors du fine-tuning du backbone |

## Vigilances & tips

Utiliser les transformations associées à `weights` pour respecter taille et normalisation attendues.
