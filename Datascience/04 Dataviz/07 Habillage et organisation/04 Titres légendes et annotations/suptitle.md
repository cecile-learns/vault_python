# suptitle

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
titre = plt.suptitle("Titre global", y=1.02)
# Alternative :
# titre = fig.suptitle("Titre global", y=1.02)
```

## Paramètres

`fig` : figure entière ; `y` : position verticale en fraction de sa hauteur.

## Fonction

Ajoute un titre commun à plusieurs graphiques.

## Vigilances & tips

`plt.suptitle("")` enlève le texte d’un titre global automatique. Vérifier le rendu avec `tight_layout()` ; `y=1.02` place le titre au-dessus du bord normal.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `x`, `y` | Position du titre dans les coordonnées de la figure. |
| `fontsize` | Taille du texte. |
| `fontweight` | Graisse, par exemple `'normal'` ou `'bold'`. |
| `ha`, `va` | Alignements horizontal et vertical autour de la position. |
| `color` | Couleur du texte. |

## Documentation officielle

[Matplotlib — `pyplot.suptitle`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.suptitle.html)
