# Girvan Newman

## Quand l'utiliser ?

Découper le graphe en retirant des arêtes structurantes.

## Pattern

```python
import networkx as nx
from networkx.algorithms.community import girvan_newman
G = nx.karate_club_graph()
communautes = [set(g) for g in next(girvan_newman(G))]
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `most_valuable_edge` | règle de choix facultative |

## Points de vigilance

- Cette méthode devient coûteuse sur les grands graphes.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.community.centrality.girvan_newman.html)
