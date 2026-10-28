# Attributs et matrice d'adjacence

## Quand l'utiliser ?

Récupérer les attributs ou la représentation matricielle.

## Pattern

```python
import networkx as nx
G = nx.Graph()
G.add_edge("A","B",weight=2.0)
poids = nx.get_edge_attributes(G,"weight")
A = nx.adjacency_matrix(G)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `name` | nom de l'attribut |
| `weight` | attribut utilisé par les algorithmes |

## Points de vigilance

- La matrice est creuse; éviter une conversion dense sur un grand graphe.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/convert.html)
