# pointplot

## Pour faire quoi ?

Comparer des estimations et leurs incertitudes par des points reliés.

## Syntaxe réutilisable

```python
ax = sns.pointplot(data=df, x='categorie', y='valeur', hue='groupe', estimator='mean', errorbar=('ci', 95), markers='o')
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x / y` | Catégorie et mesure. |
| `hue` | Sous-groupes. |
| `order / hue_order` | Ordre des groupes. |
| `estimator` | Fonction d’agrégation. |
| `errorbar` | Incertitude affichée. |
| `n_boot` | Nombre de rééchantillonnages. |
| `units` | Identifiant pour un rééchantillonnage multilevel. |
| `weights` | Poids des observations. |
| `markers` | Marqueurs des groupes. |
| `linestyles` | Styles des lignes. |
| `dodge` | Décalage entre niveaux de `hue`. |
| `capsize` | Largeur des extrémités des barres d’erreur. |
| `palette / color` | Couleurs. |
| `legend` | Réglage de la légende. |
| `ax` | Axes Matplotlib existant. |

## Fonction

Fonction de niveau Axes : elle représente des estimateurs et leurs incertitudes par des points reliés et renvoie l’`Axes`.

## Vigilances & tips

- Les lignes relient des estimations : elles n’impliquent pas nécessairement une continuité entre catégories.
- Vérifier la signification de `errorbar` avant d’interpréter l’incertitude.
- Pour voir les données brutes, ajouter un tracé de points.

## Documentation officielle

[Seaborn — `pointplot`](https://seaborn.pydata.org/generated/seaborn.pointplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
