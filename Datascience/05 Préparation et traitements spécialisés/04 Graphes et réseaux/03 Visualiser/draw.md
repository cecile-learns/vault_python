# draw

## Quand l'utiliser ?

Tracer le graphe avec Matplotlib.

## Pattern

```python
import matplotlib.pyplot as plt
import networkx as nx
G = nx.path_graph(5)
pos = nx.spring_layout(G,seed=42)
nx.draw(G,pos,with_labels=True,node_color="skyblue",node_size=900,edge_color="gray",width=1.5)
plt.show()
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `with_labels` | affiche les identifiants |
| `node_color` | couleur |
| `node_size` | taille |
| `width` | épaisseur des arêtes |

## Points de vigilance

- Réduire les labels pour un grand graphe.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/generated/networkx.drawing.nx_pylab.draw.html)
