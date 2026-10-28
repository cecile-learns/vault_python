# Construire et visualiser un graphe depuis une table

## Objectif

Passer d'une table d'arêtes à une représentation contrôlée.

## Pas à pas

1. Contrôler source, cible et attributs.
2. Choisir Graph ou DiGraph.
3. Inspecter taille et composantes.
4. Tracer avec un layout reproductible.

## Pattern complet

```python
import matplotlib.pyplot as plt
import networkx as nx
import pandas as pd
edges=pd.DataFrame({"source":["A","A","B"],"target":["B","C","C"],"poids":[2,1,3]})
G=nx.from_pandas_edgelist(edges,"source","target",edge_attr="poids")
print(G.number_of_nodes(),G.number_of_edges())
pos=nx.spring_layout(G,seed=42,weight="poids")
nx.draw(G,pos,with_labels=True,node_color="skyblue",node_size=900)
plt.show()
```

## Contrôles

- Le graphe respecte le sens métier.
- Les identifiants ne sont pas manquants.
- Les composantes isolées sont visibles.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/generated/networkx.convert_matrix.from_pandas_edgelist.html)
