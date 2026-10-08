# barplot

## Pour faire quoi ?

Comparer un estimateur, la moyenne par défaut, entre catégories.

## Syntaxe réutilisable

```python
ax = sns.barplot(data=df, x='categorie', y='valeur', hue='groupe', estimator='mean', errorbar=('ci', 95))
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x / y` | Catégorie et mesure. |
| `hue` | Sous-groupes colorés. |
| `order / hue_order` | Ordre des catégories et sous-groupes. |
| `estimator` | Fonction d’agrégation, par exemple `'mean'`, `'median'`, `sum` ou une fonction. |
| `errorbar` | Incertitude : `'ci'`, `'pi'`, `'se'`, `'sd'`, tuple ou `None`. |
| `n_boot` | Nombre de rééchantillonnages pour l’incertitude. |
| `weights` | Poids des observations. |
| `palette / color` | Palette par groupe ou couleur unique. |
| `fill` | Remplit les barres si `True`. |
| `width / gap` | Largeur des barres et espace entre elles. |
| `capsize` | Largeur des extrémités des barres d’erreur. |
| `err_kws` | Style des barres d’erreur. |
| `orient` | Orientation `'v'`, `'h'`, `'x'` ou `'y'`. |
| `legend` | Réglage de la légende. |
| `ax` | Axes Matplotlib existant. |

## Fonction

Fonction de niveau Axes : elle représente un estimateur et son incertitude par des barres et renvoie l’`Axes`.

## Vigilances & tips

- `barplot` agrège les lignes : il ne trace pas directement une valeur déjà calculée sans réfléchir à l’agrégation.
- Les barres d’erreur décrivent une incertitude ou une dispersion selon `errorbar`.
- Pour des effectifs, utiliser `countplot`.

## Documentation officielle

[Seaborn — `barplot`](https://seaborn.pydata.org/generated/seaborn.barplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
