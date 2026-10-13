# go.Box

> [!info] Bibliothèque
> Plotly

## Syntaxe

```python
trace = go.Box(x=df_athletes['Sport'], y=df_athletes['Height'], name='Taille',
               fillcolor='rgba(255, 164, 90, 0.5)', marker_size=4,
               line_color='black', line_width=1.2)
fig.add_trace(trace)
```

## Paramètres

| Paramètre | Description |
|---|---|
| `x`, `y` | Catégories et valeurs ; inverser et utiliser `orientation='h'` pour l’horizontal. |
| `name` | Nom de la trace. |
| `fillcolor` | Couleur intérieure. |
| `marker_size` | Taille des marqueurs des observations. |
| `line_color`, `line_width` | Couleur et épaisseur du contour. |
| `boxpoints` | `'outliers'`, `'suspectedoutliers'`, `'all'` ou `False.` |
| `quartilemethod` | Méthode de calcul des quartiles. |
| `notched` | Ajoute une encoche autour de la médiane. |
| `orientation` | `'v'` ou `'h'`. |

## Fonction

Crée un tracé de boîte à moustaches à ajouter à une Figure.

## Vigilances & tips

- Créer un tracé distinct par groupe si les données sont séparées.
- Pour une colonne de groupe dans un même DataFrame, `px.box()` est souvent plus court.

## Documentation officielle

- [Plotly — go.Box](https://plotly.com/python-api-reference/generated/plotly.graph_objects.Box.html)
