# boxplot

## Pour faire quoi ?

Comparer médiane, quartiles, moustaches et valeurs extrêmes entre groupes.

## Syntaxe réutilisable

```python
ax = sns.boxplot(data=df, x='categorie', y='valeur', hue='groupe', order=ordre, showmeans=True)


sns.catplot(x = 'payment_method', y = 'price', kind = 'box', col = 'category', data = df)
plt.show()
```
attention séparation avec kind = 'box', col = 'category'
## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x / y` | Catégorie et mesure quantitative. |
| `hue` | Sous-groupes. |
| `order / hue_order` | Ordre des groupes. |
| `orient` | Orientation du tracé. |
| `color / palette` | Couleur unique ou palette. |
| `fill` | Boîtes remplies si `True`. |
| `width / gap` | Largeur et espacement. |
| `whis` | Position des moustaches, `1.5` par défaut. |
| `showfliers` | Affiche les points au-delà des moustaches. |
| `showmeans` | Affiche la moyenne. |
| `notch` | Ajoute une encoche autour de la médiane. |
| `linecolor / linewidth` | Couleur et épaisseur des contours. |
| `legend` | Réglage de la légende. |
| `ax` | Axes Matplotlib existant. |

## Fonction

Fonction de niveau Axes : elle résume une distribution par catégorie avec une boîte et des moustaches et renvoie l’`Axes`.

## Vigilances & tips

- Un point au-delà des moustaches n’est pas automatiquement une erreur.
- Le boxplot résume la distribution mais masque sa forme détaillée.
- Pour voir la densité, préférer `violinplot`; pour voir les observations, ajouter `stripplot`.

## Documentation officielle

[Seaborn — `boxplot`](https://seaborn.pydata.org/generated/seaborn.boxplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
