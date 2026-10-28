# Minimiser une fonction avec PyTorch

## Pour faire quoi ?

Calculer les gradients d'une variable PyTorch et la mettre à jour avec un optimiseur.

## Script réutilisable

```python
import torch

x = torch.tensor(2.0, requires_grad=True)
optimizer = torch.optim.Adam(params=[x], lr=0.1)

for iteration in range(100):
    optimizer.zero_grad()
    valeur = x**2 - x + 2
    valeur.backward()
    optimizer.step()

print("x* :", x.item())
print("gradient :", x.grad.item())
```

## Paramètres et hyperparamètres

| Élément | Rôle | Réglage utile |
|---|---|---|
| `requires_grad=True` | suit les opérations pour calculer les gradients | activer sur les variables à optimiser |
| `lr` | learning rate | diminuer si la valeur oscille ou diverge |
| nombre d'itérations | mises à jour réalisées | augmenter si la convergence est incomplète |

## Vigilances & tips

`zero_grad()` doit précéder `backward()` : PyTorch additionne les gradients entre les itérations.

## Commandes utilisées

`torch.tensor`, `zero_grad`, `backward`, `step`, `grad`.
