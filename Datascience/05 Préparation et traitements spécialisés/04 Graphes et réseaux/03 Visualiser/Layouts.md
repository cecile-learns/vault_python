# Layouts

## Quand l'utiliser ?

Calculer la position des sommets sans modifier le graphe.

## Pattern

```python
import networkx as nx
G = nx.path_graph(6)
pos = nx.spring_layout(G,seed=42)
pos_cercle = nx.circular_layout(G)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `seed` | reproductibilité |
| `k` | espacement dans spring_layout |

## Points de vigilance

- Le layout change seulement la présentation.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/drawing.html#module-networkx.drawing.layout)
