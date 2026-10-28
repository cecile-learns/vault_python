# Perplexité et token accuracy

## Syntaxe

```python
import torch

perplexite = torch.exp(loss_moyenne)

predictions = torch.argmax(logits, dim=-1)
token_accuracy = (predictions == labels).float().mean()
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `loss_moyenne` | cross-entropy moyenne sur les tokens évalués |
| masque | exclut padding ou tokens ignorés du calcul |
| perplexité | difficulté moyenne de prédiction du prochain token |
| token accuracy | proportion de tokens prédits exactement |

## Vigilances & tips

Comparer la perplexité uniquement entre modèles évalués avec un protocole et une tokenisation compatibles.
