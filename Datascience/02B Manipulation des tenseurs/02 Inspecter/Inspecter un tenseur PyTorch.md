# Inspecter un tenseur PyTorch

## Syntaxe

```python
print(tensor.shape)
print(tensor.size())
print(tensor.dtype)
print(tensor.device)
print(tensor.requires_grad)
print(tensor.grad)
```

## Éléments

| Élément | Rôle |
|---|---|
| `shape`, `size()` | dimensions |
| `dtype` | type des valeurs |
| `device` | CPU ou GPU |
| `requires_grad` | suivi des opérations activé ou non |
| `grad` | gradient accumulé après `backward()` |

## Vigilances & tips

`grad` reste `None` tant qu'aucun `backward()` pertinent n'a été exécuté.
