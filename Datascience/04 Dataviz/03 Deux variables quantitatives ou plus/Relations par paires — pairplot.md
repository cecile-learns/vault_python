# pairplot

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Explorer rapidement toutes les relations deux à deux entre plusieurs variables, avec une distribution sur la diagonale.

## Syntaxe réutilisable

```python
g = sns.pairplot(data=df, vars=['x1', 'x2', 'x3'], hue='groupe', kind='scatter', diag_kind='hist', corner=True)
```
data = df[df['games']>60]
## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `vars` | Variables utilisées sur les deux axes. |
| `x_vars / y_vars` | Sous-ensembles distincts pour les colonnes et lignes. |
| `hue` | Variable de regroupement par couleur. |
| `hue_order` | Ordre des groupes. |
| `palette` | Palette des groupes. |
| `kind` | Tracé hors diagonale : `'scatter'`, `'kde'`, `'hist'` ou `'reg'`. |
| `diag_kind` | Tracé diagonal : `'auto'`, `'hist'`, `'kde'` ou `None`. |
| `markers` | Marqueur unique ou liste par groupe. |
| `height` | Hauteur de chaque cellule. |
| `aspect` | Rapport largeur/hauteur des cellules. |
| `corner` | N’affiche que le triangle inférieur si `True`. |
| `plot_kws` | Paramètres des tracés hors diagonale. |
| `diag_kws` | Paramètres des tracés diagonaux. |
| `grid_kws` | Paramètres de `PairGrid`. |

## Fonction

Fonction de niveau figure : elle renvoie un `PairGrid`.

## Vigilances & tips

- Le nombre de graphiques augmente au carré avec le nombre de variables.
- Limiter `vars` aux variables utiles pour garder une figure lisible.
- `corner=True` évite de répéter les mêmes relations.

## Documentation officielle

[Seaborn — `pairplot`](https://seaborn.pydata.org/generated/seaborn.pairplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
