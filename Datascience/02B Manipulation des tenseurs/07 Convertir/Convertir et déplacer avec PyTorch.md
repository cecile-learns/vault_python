# Convertir et déplacer avec PyTorch

## Syntaxe

```python
tensor = torch.tensor(array_numpy, dtype=torch.float32)
array_numpy = tensor.detach().cpu().numpy()

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
tensor = tensor.to(device)
model = model.to(device)
```

## Éléments

| Élément | Rôle |
|---|---|
| `detach()` | détache du graphe de gradients |
| `cpu()` | replace en mémoire CPU |
| `numpy()` | convertit en array NumPy |
| `to(device)` | déplace ou convertit le tenseur ou le modèle |

## Vigilances & tips

Le modèle et ses entrées doivent être sur le même device.
