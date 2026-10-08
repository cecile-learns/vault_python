# update_traces, add_annotation et for_each_annotation

## Syntaxe

```python
fig.update_traces(textposition='bottom center')
fig.add_annotation(text='Type de tracé :', showarrow=False, x=-1, y=1.06, yref='paper')
fig.for_each_annotation(lambda annotation: annotation.update(
    text=annotation.text.split('=')[1]
))
```

## Paramètres

| Paramètre | Description |
|---|---|
| `textposition` | Position du texte des tracés. |
| `selector` | Cible certains tracés, par exemple `dict(type='scatter')`. |
| `text` | Texte de l’annotation. |
| `showarrow` | Affiche une flèche si `True`. |
| `x`, `y` | Position de l’annotation. |
| `yref='paper'` | Interprète `y` comme une proportion de la figure. |
| `for_each_annotation` | Applique une fonction à chaque annotation existante. |
| `lambda annotation` | Fonction anonyme qui modifie chaque annotation. |
| `split('=')[1]` | Conserve le texte après `=` dans un titre de facette. |

## Fonction

Met à jour les tracés, ajoute une annotation ou modifie toutes les annotations existantes.

## Vigilances & tips

- Sans `selector`, `update_traces()` agit sur tous les tracés compatibles.
- `split('=')[1]` suppose que chaque titre de facette contient `=`.
- Les titres de facettes sont des annotations Plotly.

## Documentation officielle

- [Plotly — Figure.update_traces](https://plotly.com/python-api-reference/generated/plotly.graph_objects.Figure.html#plotly.graph_objects.Figure.update_traces)
- [Plotly — Figure.add_annotation](https://plotly.com/python-api-reference/generated/plotly.graph_objects.Figure.html#plotly.graph_objects.Figure.add_annotation)
- [Plotly — Figure.for_each_annotation](https://plotly.com/python-api-reference/generated/plotly.graph_objects.Figure.html#plotly.graph_objects.Figure.for_each_annotation)
