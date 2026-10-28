# shortest_path

## Quand l'utiliser ?

Calculer un chemin minimal et sa longueur.

## Pattern

```python
import networkx as nx
G = nx.Graph()
G.add_weighted_edges_from([("A","B",2),("B","C",1),("A","C",5)])
chemin = nx.shortest_path(G,"A","C",weight="weight")
distance = nx.shortest_path_length(G,"A","C",weight="weight")
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `weight=None` | nombre d'arêtes |
| `weight=weight` | somme des poids |

## Points de vigilance

- Dijkstra ne convient pas aux poids négatifs.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/algorithms/shortest_paths.html)
