# Entraîner et évaluer avec PyTorch

## Références rapides

```python
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)

model.train()
model.eval()

with torch.no_grad():
    logits = model(x_batch)
```

## Paramètres et hyperparamètres

| Élément | Rôle | Réglage utile |
|---|---|---|
| fonction de loss | objectif optimisé | selon la cible et son format |
| `lr` | learning rate | réglage prioritaire |
| `weight_decay` | régularisation L2 | augmenter si l'overfitting le justifie |
| epochs | passages sur le train | contrôler sur la validation |
| batch size | taille des lots | réglé dans le DataLoader |

## Vigilances & tips

`train()` et `eval()` modifient le comportement de Dropout et BatchNorm. `no_grad()` évite le graphe de gradients pendant l'évaluation.
