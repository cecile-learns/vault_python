# axhline

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
ligne = plt.axhline(y=valeur_reference, color="red",
                       linestyle="--", label="Référence")
```

## Paramètres

`valeur_reference` : position en unités des données ; `color`, `linestyle` : apparence ; `label` : légende.

## Fonction

Ajoute une ligne de référence horizontale.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `xmin`, `xmax` | Début et fin horizontaux, en fraction de la largeur de l’Axes entre `0` et `1`. |
| `linewidth` | Épaisseur de la ligne. |
| `alpha` | Opacité entre `0` et `1`. |
| `zorder` | Ordre de superposition. |

`y` est en unités des données, tandis que `xmin` et `xmax` sont des proportions de l’Axes.

## Documentation officielle

[Matplotlib — `pyplot.axhline`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.axhline.html)
