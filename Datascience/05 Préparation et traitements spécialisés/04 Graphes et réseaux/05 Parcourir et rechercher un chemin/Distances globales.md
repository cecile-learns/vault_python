# Distances globales

## Quand l'utiliser ?

Comparer les distances entre tous les sommets.

## Pattern

```python
import networkx as nx
G = nx.path_graph(5)
distances = dict(nx.all_pairs_shortest_path_length(G))
moyenne = nx.average_shortest_path_length(G)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `cutoff` | distance maximale éventuelle |

## Points de vigilance

- La distance moyenne exige un graphe connexe.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/algorithms/shortest_paths.html)
