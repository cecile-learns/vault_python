# px.box

> [!info] Bibliothèque
> Plotly

## Syntaxe

```python
fig = px.box(df_athletes, x='Sport', y='Height', color='Sex',
             points='outliers', title='Taille selon le sport et le sexe')
fig.show('notebook')
```

## Paramètres

| Paramètre | Description |
|---|---|
| `df_athletes` | DataFrame à représenter. |
| `x`, `y` | Catégorie et mesure quantitative. |
| `color` | Colonne qui crée des groupes colorés. |
| `orientation` | `'v'` ou `'h'` selon l’orientation voulue. |
| `points` | `'outliers'`, `'suspectedoutliers'`, `'all'` ou `False.` |
| `notched` | Ajoute une encoche autour de la médiane. |
| `boxmode` | `'group'` ou `'overlay'` avec plusieurs groupes. |
| `category_orders` | Ordre explicite des catégories. |
| `title` | Titre de la figure. |
| `template` | Thème visuel. |

## Fonction

Crée et renvoie une figure de boîtes à moustaches interactive.

## Vigilances & tips

- `px.box` évite de filtrer le DataFrame manuellement pour chaque groupe.
- Les points extrêmes ne sont pas automatiquement des erreurs de données.

## Documentation officielle

- [Plotly — px.box](https://plotly.com/python-api-reference/generated/plotly.express.box.html)
