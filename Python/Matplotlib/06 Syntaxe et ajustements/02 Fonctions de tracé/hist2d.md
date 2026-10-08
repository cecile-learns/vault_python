# hist2d

## Syntaxe

```python
hauteurs, bornes_x, bornes_y, image = ax.hist2d(valeurs_x, valeurs_y,
                                               bins=50, density=True)
```

## Paramètres

`ax` : Axes existant ; x/y : couples numériques ; `bins` : intervalles par dimension ; `density` : densité normalisée.

## Fonction

Trace un histogramme 2D ; `image` peut être passée à `fig.colorbar()`.

## Vigilances & tips

Prévoir des valeurs finies ; conserver l’appariement des lignes x/y.

Source : AED Partie 2, §6.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `range=[[xmin, xmax], [ymin, ymax]]` | Limite la plage étudiée sur les deux axes. |
| `weights` | Donne un poids à chaque couple de valeurs. |
| `cmap` | Choisit la palette utilisée pour représenter les effectifs ou densités. |
| `cmin`, `cmax` | Masquent les cellules dont le comptage est inférieur ou supérieur au seuil indiqué. |
| `vmin`, `vmax` | Fixent les valeurs associées aux extrémités de la palette. |

## Documentation officielle

[Matplotlib — `pyplot.hist2d`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.hist2d.html)
