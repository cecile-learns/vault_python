# make_subplots

## Syntaxe

```python
from plotly.subplots import make_subplots
import plotly.graph_objects as go
fig = make_subplots(rows=1, cols=2,
                    subplot_titles=('Score enseignement', 'Score recherche'))
fig.add_trace(go.Scatter(x=data.world_rank, y=data.teaching), row=1, col=1)
fig.add_trace(go.Scatter(x=data.world_rank, y=data.research), row=1, col=2)
fig.show('notebook')
```

## Paramètres

| Paramètre | Description |
|---|---|
| `rows`, `cols` | Nombre de lignes et de colonnes. |
| `subplot_titles` | Tuple de titres dans l’ordre des cellules. |
| `row`, `col` | Position de chaque tracé ; les indices commencent à 1. |
| `shared_xaxes`, `shared_yaxes` | Partage les échelles entre sous-graphiques. |
| `horizontal_spacing`, `vertical_spacing` | Espaces relatifs entre cellules. |
| `column_widths`, `row_heights` | Tailles relatives des colonnes et lignes. |
| `specs` | Description avancée du type ou de la fusion de cellules. |
| `start_cell` | `'top-left'` ou `'bottom-left'`. |

## Fonction

Crée une Figure organisée en grille. Les tracés sont ajoutés ensuite avec `fig.add_trace()`.

## Vigilances & tips

- Les indices `row` et `col` commencent à 1.
- Ne pas confondre avec `facet_row` et `facet_col` de Plotly Express.
- S’utilise souvent avec : `go.Scatter()`, `fig.add_trace()`, `fig.update_layout()`.

## Documentation officielle

- [Plotly — make_subplots](https://plotly.com/python-api-reference/generated/plotly.subplots.make_subplots.html)
- [Plotly — sous-graphiques](https://plotly.com/python/subplots/)
