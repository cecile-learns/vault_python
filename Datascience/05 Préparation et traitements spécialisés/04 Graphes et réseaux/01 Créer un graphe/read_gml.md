# read_gml

## Quand l'utiliser ?

Charger un graphe au format GML.

## Pattern

```python
import networkx as nx

G=nx.read_gml("graphe.gml")
```

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `path` | fichier GML |
| `label` | attribut utilisé comme identifiant |

## Points de vigilance

- Contrôler les identifiants et attributs après lecture.

## Documentation officielle

- [Référence](https://networkx.org/documentation/stable/reference/readwrite/generated/networkx.readwrite.gml.read_gml.html)
