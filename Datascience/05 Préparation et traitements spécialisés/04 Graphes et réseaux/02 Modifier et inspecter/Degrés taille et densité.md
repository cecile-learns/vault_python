# Degrés taille et densité

## Quand l'utiliser ?

Mesurer les connexions et la densité globale.

## Pattern

```python
import networkx as nx
G = nx.path_graph(4)
degres = dict(G.degree())
n = G.number_of_nodes()
m = G.number_of_edges()
densite = nx.density(G)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `degree` | nombre de relations |
| `density` | part des relations possibles présentes |

## Points de vigilance

- Dans un graphe orienté, distinguer in_degree et out_degree.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/generated/networkx.classes.function.density.html)
