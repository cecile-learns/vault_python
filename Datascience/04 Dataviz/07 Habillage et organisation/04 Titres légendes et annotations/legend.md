# legend

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
legende = plt.legend(loc="best")
# Alternative sur un Axes :
# legende = ax.legend(title="Séries")
```

## Paramètres

`loc` : position, ici choisie automatiquement ; `title` : titre de la légende.

## Fonction

Affiche la légende des objets ayant un `label`.

## Vigilances & tips

Définir `label` lors du tracé. Fournir seulement `labels=[…]` suppose de respecter l’ordre des objets dessinés.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `handles`, `labels` | Objets graphiques et textes à afficher explicitement. |
| `bbox_to_anchor` | Position précise de l’ancrage, souvent utilisée avec `loc`. |
| `ncols` | Nombre de colonnes de la légende. |
| `fontsize` | Taille du texte. |
| `frameon` | Affiche ou masque le cadre. |
| `facecolor`, `edgecolor` | Couleurs du fond et du contour du cadre. |
| `markerscale` | Facteur appliqué à la taille des marqueurs dans la légende. |

## Documentation officielle

[Matplotlib — `pyplot.legend`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.legend.html)
