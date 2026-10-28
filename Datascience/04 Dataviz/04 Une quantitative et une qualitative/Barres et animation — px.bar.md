# px.bar

> [!info] Bibliothèque
> Plotly

## Syntaxe

```python
fig = px.bar(df_athletes, x='Medal', y='Age', color='Team', text='Age',
             facet_row='Sex', animation_frame='Year', barmode='group')
fig.show('notebook')
```

## Paramètres

| Paramètre | Description |
|---|---|
| `df_athletes` | DataFrame à représenter. |
| `x`, `y` | Colonnes des axes. Sans `y`, la fonction compte les lignes. |
| `color` | Colonne qui sépare les barres par couleur. |
| `text` | Colonne affichée dans ou au-dessus des barres. |
| `barmode` | `'relative'` empile ; `'group'` juxtapose ; `'overlay'` superpose. |
| `orientation` | `'v'` pour vertical, `'h'` pour horizontal. |
| `facet_row`, `facet_col` | Créent une grille de panneaux. |
| `animation_frame` | Colonne qui définit une image de l’animation. |
| `animation_group` | Identifiant stable d’une barre pendant l’animation. |
| `category_orders` | Dictionnaire qui impose l’ordre des catégories. |
| `title` | Titre de la figure. |
| `template` | Thème visuel. |

## Fonction

Crée et renvoie une figure de barres interactive, éventuellement facettée ou animée.

## Vigilances & tips

- Pour retirer les commandes d’animation rencontrées dans le cours : `fig['layout'].pop('updatemenus')`.
- Sans `y`, il s’agit d’un comptage de lignes.
- S’utilise souvent avec : `fig.update_layout()`, `fig.show()`.

## Documentation officielle

- [Plotly — px.bar](https://plotly.com/python-api-reference/generated/plotly.express.bar.html)
