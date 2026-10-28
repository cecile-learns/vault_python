# Composantes connexes

## Quand l'utiliser ?

Repérer les groupes reliés entre eux.

## Pattern

```python
import networkx as nx
G = nx.Graph([(1,2),(3,4)])
est_connexe = nx.is_connected(G)
composantes = list(nx.connected_components(G))
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `connected_components` | graphe non orienté |
| `strongly_connected_components` | orienté, fortement connexe |
| `weakly_connected_components` | orienté, faiblement connexe |

## Points de vigilance

- Choisir la fonction selon l'orientation.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/algorithms/connectivity.html)
