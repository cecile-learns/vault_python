# Scaled dot-product attention

## Pattern

```python
import torch

d_k = torch.tensor(k.shape[-1], dtype=torch.float32)
scores = torch.matmul(q, k.transpose(-2, -1)) / torch.sqrt(d_k)

if masque is not None:
    scores = scores.masked_fill(masque == 0, float("-inf"))

poids_attention = torch.softmax(scores, dim=-1)
sortie_attention = torch.matmul(poids_attention, v)
```

## Paramètres et hyperparamètres

| Élément | Rôle | Réglage utile |
|---|---|---|
| `d_k` | dimension des clés | facteur d'échelle des scores |
| `dim=-1` | normalise sur les clés disponibles | conserver cet axe pour chaque requête |
| masque causal | interdit de voir les tokens futurs | nécessaire pour un modèle auto-régressif |
| nombre de têtes | attentions parallèles | augmente la diversité et le coût |

## Vigilances & tips

Une erreur de forme entre batch, séquence, tête et dimension est la cause principale des échecs d'implémentation.
