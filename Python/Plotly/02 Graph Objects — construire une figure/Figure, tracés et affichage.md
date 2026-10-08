# go.Figure, add_trace, add_traces et show

## Syntaxe

```python
import plotly.graph_objects as go
fig = go.Figure()
fig.add_trace(go.Scatter(x=valeurs_x, y=valeurs_y, name='Score'))
fig.add_traces([go.Scatter(x=valeurs_x, y=score_1, name='Score 1'),
                go.Scatter(x=valeurs_x, y=score_2, name='Score 2')])
fig.show('notebook')
```

## Paramètres

| Paramètre | Description |
|---|---|
| `go.Figure()` | Crée une figure vide. |
| `fig` | Variable qui contient la figure interactive. |
| `add_trace` | Ajoute un seul tracé. |
| `add_traces` | Ajoute une liste de tracés. |
| `row`, `col` | Cellule de destination dans une figure créée par `make_subplots()`. |
| `secondary_y` | Place le tracé sur un axe y secondaire compatible. |
| `show` | Affiche la figure ; `'notebook'` est un renderer Jupyter. |

## Fonction

Crée une Figure, y ajoute un ou plusieurs tracés et l’affiche.

## Vigilances & tips

- Une figure vide ne contient aucun graphique.
- `row` et `col` ne fonctionnent que si la figure vient de `make_subplots()`.
- S’utilise souvent avec : `go.Scatter()`, `fig.update_layout()`.

## Documentation officielle

- [Plotly — Figure](https://plotly.com/python-api-reference/generated/plotly.graph_objects.Figure.html)
- [Plotly — renderers](https://plotly.com/python/renderers/)
