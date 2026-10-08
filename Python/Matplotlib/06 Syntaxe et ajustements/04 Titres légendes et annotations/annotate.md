# annotate

## Syntaxe

```python
annotation = plt.annotate("Point à examiner", xy=(x_point, y_point),
    xytext=(x_texte, y_texte), arrowprops={"facecolor": "red"})
# Alternative :
# annotation = ax.annotate("Point", xy=(x_point, y_point))
```

## Paramètres

Texte : annotation ; 
`xy` : point visé ;
`xytext` : position du texte ; 
`arrowprops` : propriétés de la flèche.

## Fonction

Ajoute une annotation, éventuellement reliée au point par une flèche.

## Vigilances & tips

Les coordonnées présentées sont en unités des données. Sans `arrowprops`, aucune flèche.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `xycoords` | Système de coordonnées du point visé, par défaut les unités des données. |
| `textcoords` | Système de coordonnées de `xytext`, par exemple `'offset points'`. |
| `fontsize`, `color` | Taille et couleur du texte. |
| `ha`, `va` | Alignements horizontal et vertical. |
| `bbox` | Style du cadre autour du texte, sous forme de dictionnaire. |
| `annotation_clip` | Détermine si l’annotation est masquée lorsque le point est hors de l’Axes. |

## Documentation officielle

[Matplotlib — `pyplot.annotate`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.annotate.html)
