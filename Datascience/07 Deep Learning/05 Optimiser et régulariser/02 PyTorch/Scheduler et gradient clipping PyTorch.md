# Scheduler et gradient clipping PyTorch

## Syntaxe

```python
import torch

optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)
scheduler = torch.optim.lr_scheduler.ReduceLROnPlateau(
    optimizer,
    mode="min",
    factor=0.5,
    patience=5,
    min_lr=1e-6,
)

loss.backward()
torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)
optimizer.step()

scheduler.step(loss_validation)
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `mode` | sens d'amélioration | `min` pour une loss, `max` pour un score |
| `factor` | multiplicateur du learning rate | `0.5` le divise par deux |
| `patience` | epochs sans amélioration | augmenter si la métrique est bruitée |
| `min_lr` | plancher du learning rate | évite une valeur inutilisable |
| `max_norm` | norme maximale des gradients | réduire si les gradients explosent |

## Vigilances & tips

Le clipping s'effectue après `backward()` et avant `optimizer.step()`. Le scheduler reçoit la métrique de validation surveillée.
