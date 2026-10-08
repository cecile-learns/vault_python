# px.violin

## Syntaxe

```python
fig = px.violin(df_athletes, x='Medal', y='Age', color='Sex',
                box=True, points='outliers', violinmode='group')
fig.show('notebook')
```

## Paramètres

| Paramètre | Description |
|---|---|
| `df_athletes` | DataFrame à représenter. |
| `x`, `y` | Catégorie et mesure quantitative. |
| `color` | Colonne qui sépare les violons par couleur. |
| `box` | Affiche un boxplot à l’intérieur du violon. |
| `points` | `'all'`, `'outliers'`, `'suspectedoutliers'` ou `False.` |
| `violinmode` | `'group'` ou `'overlay'` pour plusieurs groupes. |
| `orientation` | `'v'` ou `'h'`. |
| `category_orders` | Ordre explicite des catégories. |
| `title` | Titre de la figure. |
| `template` | Thème visuel. |

## Fonction

Crée et renvoie une figure de violons interactive.

## Vigilances & tips

- Le violon représente une densité estimée, pas les fréquences exactes.
- `box=True` ajoute un résumé sans créer une seconde figure.

## Documentation officielle

- [Plotly — px.violin](https://plotly.com/python-api-reference/generated/plotly.express.violin.html)
