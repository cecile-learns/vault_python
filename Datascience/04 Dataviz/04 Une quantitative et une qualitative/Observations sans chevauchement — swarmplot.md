# swarmplot

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Afficher chaque observation en ajustant sa position pour éviter les chevauchements.

## Syntaxe réutilisable

```python
ax = sns.swarmplot(data=df, x='categorie', y='valeur', hue='groupe', size=4, dodge=True)
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x / y` | Catégorie et mesure. |
| `hue` | Sous-groupes. |
| `order / hue_order` | Ordre des groupes. |
| `dodge` | Sépare les niveaux de `hue`. |
| `size` | Taille des marqueurs. |
| `marker` | Forme des marqueurs. |
| `linewidth` | Épaisseur du contour. |
| `edgecolor` | Couleur du contour. |
| `palette / color` | Couleurs. |
| `warn_thresh` | Seuil de proportion de points non plaçables avant avertissement. |
| `legend` | Réglage de la légende. |
| `ax` | Axes Matplotlib existant. |

## Fonction

Fonction de niveau Axes : elle ajuste la position des observations pour limiter leur chevauchement et renvoie l’`Axes`.

## Vigilances & tips

- Le placement devient lent et difficile avec un grand effectif.
- Si des points ne peuvent pas être placés, réduire leur taille ou préférer `stripplot`.
- Peut être superposé à un boxplot ou un violinplot.

## Documentation officielle

[Seaborn — `swarmplot`](https://seaborn.pydata.org/generated/seaborn.swarmplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
