# go.Scatter

> [!info] Bibliothèque
> Plotly

## Syntaxe

```python
trace = go.Scatter(x=data.world_rank, y=data.teaching,
                   text=data.university_name, mode='lines+markers+text',
                   textposition='top center', name='Score enseignement',
                   line=dict(color='black', width=3, dash='dash'))
fig.add_trace(trace)
```

## Paramètres

| Paramètre | Description |
|---|---|
| `x`, `y` | Séquences de coordonnées de même longueur. |
| `text` | Texte associé aux points. |
| `mode` | `'lines'`, `'markers'`, `'text'` ou leur combinaison avec `+`. |
| `textposition` | Position du texte, par exemple `'top center'`. |
| `name` | Nom affiché dans la légende. |
| `line` | Dictionnaire : `color`, `width`, `dash`. |
| `marker` | Dictionnaire de style, notamment `color` et `size`. |
| `hovertemplate` | Format personnalisé du survol. |
| `opacity` | Opacité entre 0 et 1. |
| `showlegend` | Affiche ou masque l’entrée de légende. |

## Fonction

Crée un tracé de courbe, de nuage ou de texte. Il doit être ajouté à une figure.

## Vigilances & tips

- `go.Scatter()` ne l’affiche pas seul : utiliser `fig.add_trace()`.
- `mode='markers+text'` est adapté à peu de points.

## Documentation officielle

- [Plotly — go.Scatter](https://plotly.com/python-api-reference/generated/plotly.graph_objects.Scatter.html)
