# violinplot

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Comparer la forme lissée des distributions entre catégories.

## Syntaxe réutilisable

```python
ax = sns.violinplot(data=df, x='categorie', y='valeur', hue='groupe', inner='quart', cut=0)
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x / y` | Catégorie et mesure. |
| `hue` | Sous-groupes. |
| `order / hue_order` | Ordre des groupes. |
| `split` | Partage un violon entre niveaux de `hue` si `True`. |
| `inner` | Résumé intérieur : `'box'`, `'quart'`, `'point'`, `'stick'` ou `None`. |
| `density_norm` | Normalisation de largeur : `'area'`, `'count'` ou `'width'`. |
| `common_norm` | Normalisation commune aux violons. |
| `bw_method / bw_adjust` | Méthode et réglage du lissage. |
| `cut` | Extension au-delà des valeurs observées. |
| `gridsize` | Résolution de l’estimation. |
| `palette / color` | Couleurs. |
| `fill` | Remplissage. |
| `linewidth` | Épaisseur des contours. |
| `ax` | Axes Matplotlib existant. |

## Fonction

Fonction de niveau Axes : elle représente une densité estimée et peut afficher un résumé interne et renvoie l’`Axes`.

## Vigilances & tips

- Le violon représente une densité estimée, pas les observations exactes.
- `cut=0` empêche la forme de dépasser les valeurs observées.
- Avec peu de données, préférer ou superposer les points individuels.

## Documentation officielle

[Seaborn — `violinplot`](https://seaborn.pydata.org/generated/seaborn.violinplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
