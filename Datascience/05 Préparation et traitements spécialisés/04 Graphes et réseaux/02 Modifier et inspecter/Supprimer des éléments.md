# Supprimer des éléments

## Quand l'utiliser ?

Retirer un sommet ou une arête.

## Pattern

```python
import networkx as nx
G = nx.path_graph(5)
G.remove_edge(0,1)
G.remove_node(4)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `remove_node` | retire aussi ses arêtes |
| `remove_edge` | retire une relation |

## Points de vigilance

- Contrôler l'existence de l'élément avant suppression.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/classes/graph.html)
