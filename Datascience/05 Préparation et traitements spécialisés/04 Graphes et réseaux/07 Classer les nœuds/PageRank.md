# PageRank

## Quand l'utiliser ?

Classer les sommets selon l'importance transmise par leurs voisins.

## Pattern

```python
import networkx as nx
G = nx.DiGraph([("A","B"),("B","C"),("C","A"),("A","C")])
scores = nx.pagerank(G,alpha=0.85)
classement = sorted(scores.items(),key=lambda item:item[1],reverse=True)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `alpha` | facteur d'amortissement |
| `personalization` | préférence éventuelle |
| `weight` | poids |

## Points de vigilance

- PageRank exprime une importance structurelle, pas une causalité.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.link_analysis.pagerank_alg.pagerank.html)
