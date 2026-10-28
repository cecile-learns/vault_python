# Ajouter des nœuds et des arêtes

## Quand l'utiliser ?

Construire progressivement le réseau et ses attributs.

## Pattern

```python
import networkx as nx
G = nx.Graph()
G.add_node("A", groupe=1)
G.add_nodes_from(["B", "C"])
G.add_edge("A", "B", weight=2.0)
G.add_edges_from([("B", "C"), ("A", "C")])
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `add_node` | un sommet |
| `add_nodes_from` | plusieurs sommets |
| `add_edge` | une relation |
| `weight` | poids facultatif |

## Points de vigilance

- Ajouter une arête crée aussi les sommets absents.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/classes/graph.html)
