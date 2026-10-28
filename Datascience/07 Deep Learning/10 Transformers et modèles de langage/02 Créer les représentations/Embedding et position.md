# Embedding et position

## Pattern

```python
import torch
import torch.nn as nn

embedding = nn.Embedding(
    num_embeddings=taille_vocabulaire,
    embedding_dim=d_model,
)

representations = embedding(ids_tokens)

positions = torch.arange(0, longueur_max, dtype=torch.float).unsqueeze(1)
encodage_positionnel = torch.zeros((longueur_max, d_model))
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `num_embeddings` | nombre de tokens possibles | égal à la taille du vocabulaire |
| `embedding_dim` / `d_model` | dimension d'un token | augmente la capacité et le coût |
| `longueur_max` | positions disponibles | au moins la longueur des séquences traitées |

## Vigilances & tips

Les embeddings de tokens ne contiennent pas seuls l'ordre : l'information positionnelle doit être ajoutée.
