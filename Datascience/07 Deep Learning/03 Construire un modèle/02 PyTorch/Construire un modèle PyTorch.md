# Construire un modèle PyTorch

## nn.Sequential

```python
import torch.nn as nn

model = nn.Sequential(
    nn.Linear(nombre_features, 100),
    nn.ReLU(),
    nn.Linear(100, 50),
    nn.ReLU(),
    nn.Linear(50, nombre_classes),
)
```

## Classe nn.Module

```python
class ReseauDense(nn.Module):
    def __init__(self, nombre_features, nombre_classes):
        super().__init__()
        self.bloc = nn.Sequential(
            nn.Linear(nombre_features, 100),
            nn.ReLU(),
            nn.Linear(100, 50),
            nn.ReLU(),
            nn.Linear(50, nombre_classes),
        )

    def forward(self, x):
        return self.bloc(x)
```

## Paramètres et hyperparamètres

| Élément | Rôle | Réglage utile |
|---|---|---|
| `in_features` | variables reçues | égal à `X_train.shape[1]` pour un tableau |
| largeur cachée | nombre de neurones | contrôle la capacité |
| sortie finale | dimension prédite | nombre de classes, ou 1 en régression/binaire |
| activation | non-linéarité | `ReLU` dans le cours |

## Vigilances & tips

Avec `CrossEntropyLoss`, renvoyer directement les logits sans softmax final.
