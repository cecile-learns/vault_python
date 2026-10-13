# title et set_title

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
titre = plt.title("Mon graphique", fontsize=14)
# Alternative explicite :
# titre = ax.set_title("Mon graphique", fontsize=14)
```

## Paramètres

Texte : titre ; `fontsize` : taille de police en points ; `ax` : graphique ciblé.

## Fonction

Titre un graphique et retourne son objet texte.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `loc` | Position `'left'`, `'center'` ou `'right'`. |
| `pad` | Espace en points entre le titre et le haut de l’Axes. |
| `color` | Couleur du texte. |
| `fontweight` | Graisse du texte. |
| `y` | Position verticale dans les coordonnées de l’Axes. |

## Documentation officielle

- [Matplotlib — `pyplot.title`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.title.html)
- [Matplotlib — `Axes.set_title`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_title.html)
