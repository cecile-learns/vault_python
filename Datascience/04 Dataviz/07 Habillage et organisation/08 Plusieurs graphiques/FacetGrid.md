# FacetGrid

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Construire explicitement une grille de sous-graphiques selon une ou deux variables catégorielles.

## Syntaxe réutilisable

```python
g = sns.FacetGrid(df, col='categorie_colonne', row='categorie_ligne', hue='groupe', height=3, aspect=1.2)
g.map(sns.scatterplot, 'x', 'y', alpha=0.6)
g.add_legend()
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame partagé par toute la grille. |
| `row / col` | Variables qui créent les lignes et colonnes. |
| `hue` | Variable de regroupement par couleur. |
| `col_wrap` | Nombre maximal de facettes par ligne lorsque `col` est utilisé. |
| `row_order / col_order / hue_order` | Ordre explicite des modalités. |
| `height` | Hauteur de chaque facette. |
| `aspect` | Rapport largeur/hauteur de chaque facette. |
| `sharex / sharey` | Partage des axes entre facettes. |
| `margin_titles` | Place les titres de lignes dans la marge si `True`. |
| `despine` | Retire les bordures supérieure et droite si `True`. |
| `legend_out` | Réserve un espace extérieur pour la légende. |
| `dropna` | Ignore les lignes manquantes pour le facettage si `True`. |
| `map` | Applique une fonction avec les noms de colonnes comme arguments positionnels. |
| `add_legend` | Ajoute la légende construite à partir de `hue`. |

## Fonction

Crée et renvoie une grille Seaborn qui possède sa propre figure et plusieurs Axes.

## Vigilances & tips

- Quand une fonction figure-level suffit, `relplot`, `displot` ou `catplot` demandent moins de code.
- Avec `hue`, la fonction passée à `map` doit accepter une couleur et, idéalement, un libellé.
- Le titre général peut être ajouté à la figure avec `g.fig.suptitle('Titre')`.

## Documentation officielle

[Seaborn — `FacetGrid`](https://seaborn.pydata.org/generated/seaborn.FacetGrid.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
