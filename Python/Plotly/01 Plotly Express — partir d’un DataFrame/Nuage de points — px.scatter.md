# px.scatter

## Syntaxe

```python
import plotly.express as px
fig = px.scatter(df_filtre, x='budget', y='gross', color='genres',
                 size='num_voted_users', hover_name='movie_title',
                 trendline='ols', title='Audience et budget des films')
fig.show('notebook')
```

## Paramètres

| Paramètre | Description |
|---|---|
| `df_filtre` | DataFrame à représenter. |
| `x`, `y` | Colonnes des axes horizontal et vertical. |
| `color` | Colonne encodée par couleur. |
| `size` | Colonne encodée par la taille des marqueurs. |
| `text` | Colonne affichée près des points. |
| `hover_name` | Texte principal affiché au survol. |
| `facet_col` | Crée une colonne de panneaux par modalité. |
| `marginal_x`, `marginal_y` | Ajoutent un résumé de distribution, par exemple `'box'`. |
| `trendline='ols'` | Ajoute une droite des moindres carrés ; nécessite `statsmodels`. |
| `title` | Titre de la figure. |
| `labels` | Dictionnaire pour renommer les libellés visibles. |
| `template` | Thème visuel, par exemple `'plotly_white'`. |

## Fonction

Crée et renvoie une figure interactive représentant une relation entre deux variables.

## Vigilances & tips

- `trendline='ols'` aide à explorer une tendance ; il ne remplace pas l’évaluation d’un modèle.
- Avec beaucoup de points, éviter de cumuler `text` et `hover_name`.
- S’utilise souvent avec : `fig.update_traces()`, `fig.update_layout()`, `fig.show()`.

## Documentation officielle

- [Plotly — px.scatter](https://plotly.com/python-api-reference/generated/plotly.express.scatter.html)
