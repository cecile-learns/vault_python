# Transformer un tenseur PyTorch

## Syntaxe

```python
t_remodele = torch.reshape(tensor, (lignes, colonnes))
t_transpose = torch.transpose(tensor, 0, 1)
t_avec_dimension = torch.unsqueeze(tensor, dim=0)
```

## Paramètres

| Paramètre | Rôle |
|---|---|
| nouvelle forme | dimensions finales de `reshape` |
| `dim0`, `dim1` | dimensions échangées |
| `dim` | position de la nouvelle dimension de taille 1 |

## Vigilances & tips

Le nombre total de valeurs doit rester identique avec `reshape`.
