# colorbar

## Syntaxe

```python
echelle = fig.colorbar(image)
```

## Paramètres

`fig` : figure ; `image` : objet coloré, par exemple quatrième retour de `ax.hist2d()`.

## Fonction

Ajoute l’échelle correspondant aux couleurs du tracé.

## Vigilances & tips

Ne pas passer le tableau des hauteurs à la place de l’objet graphique.

Source : AED Partie 2, §6.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `ax` | Axes auquel la place de la barre de couleurs est rattachée. |
| `label` | Texte décrivant la grandeur représentée. |
| `location` | Côté d’affichage : `'left'`, `'right'`, `'top'` ou `'bottom'`. |
| `orientation` | Orientation `'vertical'` ou `'horizontal'` ; doit être compatible avec `location`. |
| `shrink` | Réduit la longueur de la barre par un facteur multiplicatif. |
| `aspect` | Rapport entre sa longueur et son épaisseur. |
| `pad` | Espace entre l’Axes et la barre de couleurs. |
| `ticks` | Positions des graduations sur l’échelle de couleurs. |

## Documentation officielle

[Matplotlib — `Figure.colorbar`](https://matplotlib.org/stable/api/_as_gen/matplotlib.figure.Figure.colorbar.html)
