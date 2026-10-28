# BFS et DFS

## Quand l'utiliser ?

Parcourir en largeur ou en profondeur.

## Pattern

```python
import networkx as nx
G = nx.path_graph(6)
bfs = list(nx.bfs_edges(G,source=0))
dfs = list(nx.dfs_edges(G,source=0))
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `source` | sommet de départ |
| `depth_limit` | profondeur maximale |

## Points de vigilance

- BFS donne un plus court chemin en nombre d'arêtes sans poids.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/algorithms/traversal.html)
