# scatterplot

## Pour faire quoi ?

Afficher la relation entre deux variables quantitatives et encoder d’autres variables par couleur, taille ou marqueur.

## Syntaxe réutilisable

```python
ax = sns.scatterplot(data=df, x='x', y='y', hue='groupe', size='importance', style='type', alpha=0.7)
```
data = df[df['games']>60]
## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x` | Variable quantitative horizontale. |
| `y` | Variable quantitative verticale. |
| `hue` | Variable encodée par couleur. |
| `size` | Variable encodée par taille. |
| `style` | Variable encodée par forme de marqueur. |
| `palette` | Palette ou dictionnaire de couleurs. |
| `hue_order` | Ordre des modalités de `hue`. |
| `hue_norm` | Normalisation d’un `hue` numérique. |
| `sizes` | Intervalle `(min, max)` ou dictionnaire des tailles. |
| `size_norm` | Normalisation d’une variable de taille numérique. |
| `markers` | Liste, dictionnaire ou booléen pour les marqueurs. |
| `alpha` | Opacité des points. |
| `legend` | Légende : `'auto'`, `'brief'`, `'full'` ou `False`. |
| `ax` | Axes Matplotlib existant. |

## Fonction

Fonction de niveau Axes : elle renvoie l’`Axes` contenant le nuage de points.
data=df peut aussi devenir conditionnelle data = df[df['games']>60]
## Vigilances & tips

- Pour beaucoup de points, réduire `alpha` et la taille transmise avec `s=`.
- Une variable numérique dans `hue` utilise une palette continue ; une variable catégorielle utilise des couleurs distinctes.
- Les paramètres Matplotlib des marqueurs, comme `edgecolor` et `linewidth`, peuvent aussi être transmis.

## Documentation officielle

[Seaborn — `scatterplot`](https://seaborn.pydata.org/generated/seaborn.scatterplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
