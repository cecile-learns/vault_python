# Cycles arbres et forêts

## Quand l'utiliser ?

Vérifier cycles et structure arborescente.

## Pattern

```python
import networkx as nx
G = nx.cycle_graph(4)
cycles = nx.cycle_basis(G)
est_arbre = nx.is_tree(G)
est_foret = nx.is_forest(G)
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `cycle_basis` | base de cycles |
| `is_tree` | connexe et sans cycle |

## Points de vigilance

- Les fonctions diffèrent pour les graphes orientés.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/algorithms/cycles.html)
