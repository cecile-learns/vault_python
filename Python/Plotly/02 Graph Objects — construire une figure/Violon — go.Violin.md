# go.Violin

## Syntaxe

```python
trace = go.Violin(x=df_athletes['Medal'], y=df_athletes['Age'], name='Âge',
                  fillcolor='lightseagreen', line_color='black',
                  box_visible=True, meanline_visible=True)
fig.add_trace(trace)
fig.update_layout(violinmode='group')
```

## Paramètres

| Paramètre | Description |
|---|---|
| `x`, `y` | Catégories et valeurs quantitatives. |
| `name` | Nom de la trace. |
| `fillcolor` | Couleur de remplissage. |
| `line_color` | Couleur du contour. |
| `box_visible` | Affiche un boxplot interne. |
| `meanline_visible` | Affiche la moyenne. |
| `points` | `'all'`, `'outliers'`, `'suspectedoutliers'` ou `False.` |
| `side` | `'positive'`, `'negative'` ou `'both'`. |
| `bandwidth` | Largeur de lissage de la densité. |
| `violinmode` | Paramètre de `update_layout` : `'group'` ou `'overlay'`. |

## Fonction

Crée un tracé de violon à ajouter à une Figure.

## Vigilances & tips

- Le violon est une estimation lissée de densité.
- `px.violin()` évite les filtrages répétés quand les groupes proviennent d’une colonne.

## Documentation officielle

- [Plotly — go.Violin](https://plotly.com/python-api-reference/generated/plotly.graph_objects.Violin.html)
