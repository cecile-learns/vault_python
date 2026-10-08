# go.Pie

## Syntaxe

```python
trace = go.Pie(labels=df_livres['genre'], values=df_livres['count'],
               pull=[0, 0.1, 0],
               marker_colors=['lightblue', 'mediumturquoise', 'lightgreen'])
fig.add_trace(trace)
```

## Paramètres

| Paramètre | Description |
|---|---|
| `labels` | Libellés des parts. |
| `values` | Valeurs associées aux parts. |
| `pull` | Décalage radial de chaque part. |
| `marker_colors` | Couleurs des parts. |
| `marker_line` | Dictionnaire de contour. |
| `textinfo` | Informations imprimées, par exemple `'label+percent'`. |
| `hole` | Trou central entre 0 et 1. |
| `sort` | Trie les parts par valeur. |
| `direction` | `'clockwise'` ou `'counterclockwise'`. |
| `name` | Nom de la trace. |

## Fonction

Crée un tracé circulaire à ajouter à une Figure.

## Vigilances & tips

- Éviter un grand nombre de parts : les barres sont souvent plus lisibles.
- `pull` doit suivre l’ordre des parts.

## Documentation officielle

- [Plotly — go.Pie](https://plotly.com/python-api-reference/generated/plotly.graph_objects.Pie.html)

```Python
# Calcul de l'occurence de chaque valeur de User Rating
l = data_2019['User Rating'].unique()
occurence = []
for i in range(len(l)) :
    occurence.append(data_2019.loc[data_2019['User Rating'] == l[i]].count()[0])


fig_1 = go.Figure()

colors = ['lightblue','beige','lightseagreen','antiquewhite','cadetblue','darkorange','goldenrod']

fig_1.add_trace(go.Pie(labels = l,
                       values = occurence,
                       marker_line = dict(color = 'black', width = 1.5), # Couleur et épaisseur de la ligne 
                       marker_colors = colors,  # Couleur de chaque partie du camembert 
                       pull = [0.2,0,0,0,0,0,0] # Partie à éloigner du camembert 
                      ))

fig_1.show("notebook")
```
![[Pasted image 20260922144814.png]]