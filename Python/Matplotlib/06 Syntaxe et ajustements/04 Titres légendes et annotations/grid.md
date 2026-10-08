# grid

## Syntaxe

```python
plt.grid(True, axis="y", linestyle="--", alpha=0.7)
# Variante explicite rencontrée :
# ax.yaxis.grid(True, linestyle="--", alpha=0.7)
```

## Paramètres

`True` : afficher ; `axis="y"` : lignes horizontales ; `linestyle` : tirets ; `alpha` : opacité.

## Fonction

Affiche une grille pour faciliter la lecture.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `visible` | Affiche ou masque la grille ; omis, l’appel bascule son état si aucun style n’est fourni. |
| `which` | Cible les graduations `'major'`, `'minor'` ou `'both'`. |
| `axis` | Cible `'x'`, `'y'` ou `'both'`. |
| `color` | Couleur des lignes. |
| `linewidth` | Épaisseur des lignes. |
| `linestyle`, `alpha` | Style et opacité des lignes. |

## Documentation officielle

[Matplotlib — `pyplot.grid`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.grid.html)
