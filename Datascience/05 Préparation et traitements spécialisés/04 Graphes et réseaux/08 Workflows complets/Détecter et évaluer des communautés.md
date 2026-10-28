# Détecter et évaluer des communautés

## Objectif

Produire une partition puis contrôler sa qualité.

## Pas à pas

1. Préparer le graphe et les poids.
2. Tester une partition Louvain.
3. Calculer modularité et qualité.
4. Interpréter les groupes.

## Pattern complet

```python
import networkx as nx
G=nx.karate_club_graph()
communautes=nx.community.louvain_communities(G,seed=42)
modularite=nx.community.modularity(G,communautes)
coverage,performance=nx.community.partition_quality(G,communautes)
print(modularite,coverage,performance)
```

## Contrôles

- Chaque sommet appartient à un groupe.
- La résolution a été testée.
- Le découpage a un sens métier.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/algorithms/community.html)
