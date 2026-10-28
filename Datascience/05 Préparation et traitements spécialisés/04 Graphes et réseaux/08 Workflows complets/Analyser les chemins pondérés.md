# Analyser les chemins pondérés

## Objectif

Trouver un itinéraire minimal selon un coût explicite.

## Pas à pas

1. Définir le sens du poids.
2. Conserver le poids sur les arêtes.
3. Le passer aux fonctions de chemin.
4. Relire la somme obtenue.

## Pattern complet

```python
import networkx as nx
G=nx.Graph()
G.add_weighted_edges_from([("A","B",2),("B","D",2),("A","C",1),("C","D",5)])
chemin=nx.shortest_path(G,"A","D",weight="weight")
cout=nx.shortest_path_length(G,"A","D",weight="weight")
print(chemin,cout)
```

## Contrôles

- Poids numériques et de signe compatible.
- Direction des arêtes correcte.
- Chemin interprétable.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/algorithms/shortest_paths.html)
