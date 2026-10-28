# Boucle complète PyTorch

## Pour faire quoi ?

Entraîner puis évaluer un modèle de classification avec des DataLoaders.

## Script réutilisable

```python
import torch
import torch.nn as nn

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model = model.to(device)
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)

for epoch in range(20):
    model.train()
    perte_train = 0.0

    for X_batch, y_batch in train_loader:
        X_batch, y_batch = X_batch.to(device), y_batch.to(device)
        optimizer.zero_grad()
        logits = model(X_batch)
        loss = criterion(logits, y_batch)
        loss.backward()
        optimizer.step()
        perte_train += loss.item() * X_batch.size(0)

    model.eval()
    corrects, total = 0, 0
    with torch.no_grad():
        for X_batch, y_batch in test_loader:
            X_batch, y_batch = X_batch.to(device), y_batch.to(device)
            predictions = torch.argmax(model(X_batch), dim=1)
            corrects += (predictions == y_batch).sum().item()
            total += y_batch.size(0)

    print(
        f"Epoch {epoch + 1} — "
        f"loss={perte_train / len(train_loader.dataset):.4f} — "
        f"accuracy={corrects / total:.4f}"
    )
```

## Paramètres et hyperparamètres

| Paramètre | Effet |
|---|---|
| learning rate | vitesse et stabilité |
| epochs | durée d'apprentissage |
| batch size | mémoire et bruit du gradient |
| loss | objectif adapté à la cible |

## Vigilances & tips

Employer une validation pour régler le modèle et réserver le test à l'évaluation finale.
