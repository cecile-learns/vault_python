# ylabel et set_ylabel

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
texte = plt.ylabel("Nom et unité", fontsize=11)
# Alternative :
# texte = ax.set_ylabel("Nom et unité", fontsize=11)
```

## Paramètres

Texte : nom de variable et unité ; `fontsize` : taille en points.

## Fonction

Nomme l’axe y et retourne son objet texte.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `labelpad` | Espace en points entre le nom et l’axe y. |
| `loc` | Position `'bottom'`, `'center'` ou `'top'`. |
| `color` | Couleur du texte. |
| `fontweight` | Graisse du texte. |

## Documentation officielle

- [Matplotlib — `pyplot.ylabel`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.ylabel.html)
- [Matplotlib — `Axes.set_ylabel`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_ylabel.html)
