# Évaluer une partition

## Quand l'utiliser ?

Mesurer modularité, couverture et performance.

## Pattern

```python
import networkx as nx
G = nx.karate_club_graph()
communautes = nx.community.louvain_communities(G,seed=42)
modularite = nx.community.modularity(G,communautes)
coverage, performance = nx.community.partition_quality(G,communautes)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `modularity` | séparation interne/externe |
| `partition_quality` | coverage et performance |

## Points de vigilance

- Une bonne valeur ne remplace pas l'interprétation métier.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/algorithms/community.html)
