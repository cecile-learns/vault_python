# text

## Syntaxe

```python
texte = plt.text(position_x, position_y, "Valeur", ha="center", va="bottom")
```

## Paramètres

x/y : position dans les unités des données ; texte : contenu ; `ha`, `va` : alignements horizontal et vertical.

## Fonction

Ajoute du texte à une position choisie.

## Vigilances & tips

Pour une valeur sur une barre : placer y au-dessus de sa hauteur ; le décalage doit être adapté à l’échelle.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `fontsize` | Taille du texte. |
| `color` | Couleur du texte. |
| `rotation` | Rotation en degrés. |
| `fontweight` | Graisse du texte. |
| `bbox` | Cadre autour du texte, décrit par un dictionnaire. |
| `transform=ax.transAxes` | Interprète x et y comme proportions de l’Axes plutôt que comme unités des données. |

Avec `ax.transAxes`, `(0, 0)` est le coin inférieur gauche et `(1, 1)` le coin supérieur droit de l’Axes.

## Documentation officielle

[Matplotlib — `pyplot.text`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.text.html)
