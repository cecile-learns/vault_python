# Générateurs et composition

## Quand l'utiliser ?

Créer des graphes usuels puis les combiner.

## Pattern

```python
import networkx as nx
G1 = nx.path_graph(4)
G2 = nx.path_graph([3,4,5])
G = nx.compose(G1,G2)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `path_graph` | graphe en chaîne |
| `compose` | union de deux graphes |

## Points de vigilance

- Les attributs du second graphe gagnent en cas de conflit.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/algorithms/operators.html)
