# catplot

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Créer un graphique catégoriel et le répartir en facettes avec une interface unique.

## Syntaxe réutilisable

```python
g = sns.catplot(data=df, x='categorie', y='valeur', hue='groupe', col='facette', kind='box', height=4, aspect=1.1)
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x / y` | Catégorie et mesure selon le type de tracé. |
| `hue` | Sous-groupes colorés. |
| `row / col` | Variables de facettage. |
| `col_wrap` | Nombre maximal de facettes par ligne. |
| `kind` | `'strip'`, `'swarm'`, `'box'`, `'violin'`, `'boxen'`, `'point'`, `'bar'` ou `'count'`. |
| `order / hue_order` | Ordre des catégories. |
| `row_order / col_order` | Ordre des facettes. |
| `height` | Hauteur de chaque facette. |
| `aspect` | Rapport largeur/hauteur de chaque facette. |
| `sharex / sharey` | Partage des axes entre facettes. |
| `legend` | Affiche la légende. |
| `legend_out` | Place la légende hors de la grille. |
| `facet_kws` | Réglages supplémentaires de `FacetGrid`. |
| `palette` | Palette des groupes. |

## Fonction

Fonction de niveau figure : elle renvoie un `FacetGrid` et appelle la fonction catégorielle choisie par `kind`.

## Vigilances & tips

- Ne pas utiliser `ax=`.
- Les paramètres propres au type choisi sont transmis à la fonction sous-jacente.
- Pour `kind='count'`, une seule variable de catégorie suffit et aucune mesure `y` n’est nécessaire.

## Documentation officielle

[Seaborn — `catplot`](https://seaborn.pydata.org/generated/seaborn.catplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
