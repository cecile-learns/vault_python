# Arbre couvrant minimum

## Quand l'utiliser ?

Relier les sommets avec un poids total minimal.

## Pattern

```python
import networkx as nx
G = nx.Graph()
G.add_weighted_edges_from([("A","B",2),("B","C",1),("A","C",5)])
T = nx.minimum_spanning_tree(G,algorithm="kruskal",weight="weight")
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `algorithm` | kruskal, prim ou boruvka |
| `weight` | nom du coût |

## Points de vigilance

- Clarifier si le poids représente un coût, une distance ou autre.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/algorithms/tree.html)
