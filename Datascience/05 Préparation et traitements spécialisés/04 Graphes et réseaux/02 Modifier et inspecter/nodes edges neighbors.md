# nodes edges neighbors

## Quand l'utiliser ?

Examiner sommets, relations et voisinage.

## Pattern

```python
import networkx as nx
G = nx.path_graph(4)
noeuds = list(G.nodes(data=True))
aretes = list(G.edges(data=True))
voisins = list(G.neighbors(1))
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `data=True` | inclut les attributs |

## Points de vigilance

- Les vues NetworkX sont dynamiques; convertir en liste pour les figer.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/classes/index.html)
