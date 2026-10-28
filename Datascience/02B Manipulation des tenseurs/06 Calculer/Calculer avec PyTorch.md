# Calculer avec PyTorch

## Syntaxe

```python
produit = torch.matmul(tensor_a, tensor_b)
inverse = torch.linalg.inv(matrice)
moyenne = torch.mean(tensor, dim=0)
norme = torch.norm(tensor)
```

## Paramètres

| Paramètre | Rôle |
|---|---|
| `dim` | dimension réduite |
| `keepdim=True` | conserve une dimension de taille 1 |

## Vigilances & tips

`matmul` exige des dimensions internes compatibles. `linalg.inv` attend une matrice carrée inversible.
