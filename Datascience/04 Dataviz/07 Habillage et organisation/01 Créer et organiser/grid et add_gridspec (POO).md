# add_gridspec

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
plt.grid()

grille = fig.add_gridspec(2, 3, hspace=0.4, wspace=0.35)
```

## Paramètres

`fig` : Figure existante ; `2, 3` : lignes et colonnes ; `hspace`, `wspace` : espacements relatifs vertical et horizontal.

## Fonction

Retourne une grille de placement pour ajouter des graphiques.

## Vigilances & tips

S’utilise avec `fig.add_subplot()`.

Source : Data Visualisation, partie 4.

## Paramètres courants complémentaires de `add_gridspec`

| Paramètre | Effet |
|---|---|
| `nrows`, `ncols` | Nombre de lignes et de colonnes de la grille. |
| `width_ratios` | Largeur relative de chaque colonne. |
| `height_ratios` | Hauteur relative de chaque ligne. |
| `left`, `right`, `bottom`, `top` | Position des bords de la grille dans la figure. |
| `wspace`, `hspace` | Espacement relatif entre colonnes et entre lignes. |

`plt.grid()` agit sur la grille visuelle du graphique ; `fig.add_gridspec()` construit une grille de placement pour plusieurs Axes. Ce sont deux usages différents malgré le mot « grid ».

## Documentation officielle

- [Matplotlib — `Figure.add_gridspec`](https://matplotlib.org/stable/api/_as_gen/matplotlib.figure.Figure.add_gridspec.html)
- [Matplotlib — `pyplot.grid`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.grid.html)
