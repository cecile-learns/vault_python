# Louvain

## Quand l'utiliser ?

Trouver une partition à forte modularité.

## Pattern

```python
import networkx as nx
G = nx.karate_club_graph()
communautes = nx.community.louvain_communities(G,resolution=1,seed=42)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `resolution` | taille relative des groupes |
| `weight` | poids |
| `seed` | reproductibilité |

## Points de vigilance

- Tester plusieurs résolutions et interpréter les groupes.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.community.louvain.louvain_communities.html)
