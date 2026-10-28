# Graph et DiGraph

## Quand l'utiliser ?

Créer un graphe non orienté ou orienté.

## Pattern

```python
import networkx as nx
G = nx.Graph()
D = nx.DiGraph()
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `nx.Graph()` | relations sans direction |
| `nx.DiGraph()` | relations orientées |

## Points de vigilance

- Choisir le type selon le sens métier des relations.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/classes/index.html)
