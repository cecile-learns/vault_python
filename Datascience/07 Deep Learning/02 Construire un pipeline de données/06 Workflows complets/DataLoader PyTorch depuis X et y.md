# DataLoader PyTorch depuis X et y

## Pour faire quoi ?

Convertir des tableaux en tenseurs puis fournir des mini-lots à une boucle PyTorch.

## Script réutilisable

```python
import torch
from torch.utils.data import DataLoader, TensorDataset

X_train_tensor = torch.tensor(X_train, dtype=torch.float32)
y_train_tensor = torch.tensor(y_train, dtype=torch.long)

train_loader = DataLoader(
    TensorDataset(X_train_tensor, y_train_tensor),
    batch_size=32,
    shuffle=True,
)

test_loader = DataLoader(
    TensorDataset(
        torch.tensor(X_test, dtype=torch.float32),
        torch.tensor(y_test, dtype=torch.long),
    ),
    batch_size=32,
    shuffle=False,
)
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `dtype` de `X` | type des entrées | généralement `float32` |
| `dtype` de `y` | type de cible | `long` avec `CrossEntropyLoss`, `float32` en régression |
| `batch_size` | observations par mise à jour | tester 16, 32 ou 64 |
| `shuffle` | mélange les observations | `True` pour train, `False` pour test |

## Vigilances & tips

Contrôler forme et type du premier batch avant l'entraînement.
