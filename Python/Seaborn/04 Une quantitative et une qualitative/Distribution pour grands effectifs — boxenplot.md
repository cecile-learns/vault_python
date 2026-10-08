sn# boxenplot

## Pour faire quoi ?

Montrer davantage de quantiles qu’un boxplot, surtout avec un effectif important.

## Syntaxe réutilisable

```python
ax = sns.boxenplot(data=df, x='categorie', y='valeur', hue='groupe', k_depth='tukey')
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x / y` | Catégorie et mesure. |
| `hue` | Sous-groupes. |
| `order / hue_order` | Ordre des groupes. |
| `k_depth` | Profondeur des niveaux : `'tukey'`, `'proportion'`, `'trustworthy'` ou `'full'`. |
| `outlier_prop` | Proportion attendue de valeurs extrêmes. |
| `trust_alpha` | Confiance utilisée avec `k_depth='trustworthy'`. |
| `showfliers` | Affiche les valeurs extrêmes. |
| `width_method` | Variation des largeurs : `'exponential'`, `'linear'` ou `'area'`. |
| `palette / color` | Couleurs. |
| `fill` | Remplit les boîtes. |
| `linewidth` | Épaisseur des contours. |
| `width / gap` | Largeur et espacement. |
| `ax` | Axes Matplotlib existant. |

## Fonction

Fonction de niveau Axes : elle affiche une succession de quantiles plus détaillée qu’un boxplot et renvoie l’`Axes`.

## Vigilances & tips

- Cette représentation est plus informative lorsque chaque groupe contient beaucoup d’observations.
- Avec de petits effectifs, les quantiles profonds sont instables.
- Comparer les groupes sur une même échelle.

## Documentation officielle

[Seaborn — `boxenplot`](https://seaborn.pydata.org/generated/seaborn.boxenplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
