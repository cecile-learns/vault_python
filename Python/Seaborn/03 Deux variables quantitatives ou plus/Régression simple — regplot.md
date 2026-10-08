# regplot

## Pour faire quoi ?

Afficher un nuage de points avec une courbe de régression sur un seul Axes.

## Syntaxe réutilisable

```python
ax = sns.regplot(data=df, x='x', y='y', order=1, ci=95, scatter_kws={'alpha': 0.5})
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x / y` | Variables quantitative explicative et cible. |
| `order` | Degré d’une régression polynomiale. |
| `logistic` | Ajuste une régression logistique si `True`. |
| `lowess` | Utilise un lissage LOWESS si `True`. |
| `robust` | Utilise une régression robuste si `True`. |
| `ci` | Niveau de l’intervalle de confiance ou `None`. |
| `n_boot` | Nombre de rééchantillonnages pour l’intervalle. |
| `x_jitter / y_jitter` | Décalage aléatoire visuel des points. |
| `scatter` | Affiche les observations si `True`. |
| `fit_reg` | Affiche la courbe ajustée si `True`. |
| `scatter_kws` | Dictionnaire de style des points. |
| `line_kws` | Dictionnaire de style de la courbe. |
| `ax` | Axes Matplotlib existant. |

## Fonction

Fonction de niveau Axes : elle ajuste le modèle destiné à la visualisation et renvoie l’`Axes`.

## Vigilances & tips

- La courbe montre une association, pas une causalité.
- Un ordre polynomial élevé peut surajuster les données.
- Pour évaluer un modèle prédictif, utiliser une démarche de modélisation distincte.

## Documentation officielle

[Seaborn — `regplot`](https://seaborn.pydata.org/generated/seaborn.regplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
