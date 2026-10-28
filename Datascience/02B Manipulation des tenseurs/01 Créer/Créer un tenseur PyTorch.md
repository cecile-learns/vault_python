# Créer un tenseur PyTorch

## Syntaxe

```python
import torch

t_depuis_donnees = torch.tensor(
    donnees,
    dtype=torch.float32,
    device="cpu",
    requires_grad=False,
)

t_zeros = torch.zeros((3, 4), dtype=torch.float32)
t_uns = torch.ones((3, 4), dtype=torch.float32)
t_normal = torch.randn((3, 4))
t_entiers = torch.randint(low=0, high=11, size=(40,))
```

## Paramètres

| Paramètre | Rôle |
|---|---|
| `data` | liste, tuple, scalaire ou array converti |
| `dtype` | type numérique : `float32`, `int64`, etc. |
| `device` | matériel : `cpu` ou `cuda` |
| `requires_grad` | conserve le graphe nécessaire aux gradients |
| `size` | dimensions du tenseur créé |
| `low`, `high` | borne basse incluse et borne haute exclue de `randint` |

## Vigilances & tips

Une cible utilisée avec `CrossEntropyLoss` est généralement en `torch.long`.
