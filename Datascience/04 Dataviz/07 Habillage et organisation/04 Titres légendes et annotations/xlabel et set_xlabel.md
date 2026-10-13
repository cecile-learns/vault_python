# xlabel et set_xlabel

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
texte = plt.xlabel("Nom et unité", fontsize=11)
# Alternative :
# texte = ax.set_xlabel("Nom et unité", fontsize=11)
```

## Paramètres

Texte : nom de variable et unité ; `fontsize` : taille en points.

## Fonction

Nomme l’axe x et retourne son objet texte.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `labelpad` | Espace en points entre le nom et l’axe x. |
| `loc` | Position `'left'`, `'center'` ou `'right'`. |
| `color` | Couleur du texte. |
| `fontweight` | Graisse du texte. |

## Documentation officielle

- [Matplotlib — `pyplot.xlabel`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.xlabel.html)
- [Matplotlib — `Axes.set_xlabel`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_xlabel.html)
