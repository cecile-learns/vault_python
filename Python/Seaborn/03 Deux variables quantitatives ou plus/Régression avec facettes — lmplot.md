# lmplot

## Pour faire quoi ?

Comparer des régressions entre groupes ou facettes.

## Syntaxe réutilisable

```python
g = sns.lmplot(data=df, x='x', y='y', hue='groupe', col='categorie', height=4, aspect=1.1)
```

## Paramètres courants

| Paramètre     | Description                                                       |
| ------------- | ----------------------------------------------------------------- |
| `data`        | DataFrame source.                                                 |
| `x / y`       | Variables de la régression.                                       |
| `hue`         | Groupes colorés et ajustés séparément.                            |
| `row / col`   | Variables de facettage.                                           |
| `col_wrap`    | Nombre maximal de facettes par ligne.                             |
| `height`      | Hauteur de chaque facette.                                        |
| `aspect`      | Rapport largeur/hauteur de chaque facette.                        |
| `order`       | Degré polynomial. courbe de regression quadratique                |
| `logistic`    | Régression logistique si `True`.                                  |
| `lowess`      | Lissage LOWESS si `True`. Courbe plus lisse par regression locale |
| `robust`      | Régression robuste si `True`.                                     |
| `ci`          | Niveau de confiance ou `None`.                                    |
| `scatter_kws` | Style des points.                                                 |
| `line_kws`    | Style des courbes.                                                |
| `facet_kws`   | Réglages de la grille.                                            |
|               |                                                                   |

## Fonction

Fonction de niveau figure : elle associe `regplot` à un `FacetGrid` et renvoie cette grille.

## Vigilances & tips

- Ne pas utiliser `ax=`.
- Chaque groupe ou facette est ajusté séparément.
- La largeur d’une facette vaut `height * aspect`.

## Documentation officielle

[Seaborn — `lmplot`](https://seaborn.pydata.org/generated/seaborn.lmplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
