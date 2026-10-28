# Charger des dossiers d'images avec PyTorch

## Pour faire quoi ?

Créer un Dataset et un DataLoader lorsqu'une classe correspond à chaque sous-dossier.

## Script réutilisable

```python
from torch.utils.data import DataLoader
from torchvision import datasets, transforms

transform_train = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor(),
    transforms.Normalize(
        mean=[0.485, 0.456, 0.406],
        std=[0.229, 0.224, 0.225],
    ),
])

train_dataset = datasets.ImageFolder(
    root="Training",
    transform=transform_train,
)

train_loader = DataLoader(
    train_dataset,
    batch_size=32,
    shuffle=True,
)

print(train_dataset.class_to_idx)
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `Resize` | forme commune | reprendre la taille attendue par le modèle |
| `mean`, `std` | normalisation par canal | utiliser celles du modèle préentraîné |
| `batch_size` | images par lot | réduire en cas de mémoire insuffisante |
| `shuffle` | mélange les images | `True` uniquement pour le train |

## Vigilances & tips

Vérifier `class_to_idx` avant l'entraînement : `ImageFolder` déduit les classes des noms de dossiers.
