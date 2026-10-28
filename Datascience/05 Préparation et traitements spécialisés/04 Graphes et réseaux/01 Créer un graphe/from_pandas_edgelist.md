# from_pandas_edgelist

## Quand l'utiliser ?

Transformer une table source-cible en graphe.

## Pattern

```python
import networkx as nx
import pandas as pd
edges = pd.DataFrame({"source":["A","B"],"target":["B","C"],"poids":[1.2,2.0]})
G = nx.from_pandas_edgelist(edges, source="source", target="target", edge_attr="poids")
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `source` | colonne de départ |
| `target` | colonne d'arrivée |
| `edge_attr` | attributs conservés |
| `create_using` | type de graphe |

## Points de vigilance

- Vérifier doublons, valeurs manquantes et orientation.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/generated/networkx.convert_matrix.from_pandas_edgelist.html)
