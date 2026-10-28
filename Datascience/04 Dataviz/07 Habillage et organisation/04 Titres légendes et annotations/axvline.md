# axvline

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
ligne = plt.axvline(x=valeur_reference, color="red",
                       linestyle="--", label="Référence")
```

## Paramètres

`valeur_reference` : position en unités des données ; `color`, `linestyle` : apparence ; `label` : légende.

## Fonction

Ajoute une ligne de référence verticale.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `ymin`, `ymax` | Début et fin verticaux, en fraction de la hauteur de l’Axes entre `0` et `1`. |
| `linewidth` | Épaisseur de la ligne. |
| `alpha` | Opacité entre `0` et `1`. |
| `zorder` | Ordre de superposition. |

`x` est en unités des données, tandis que `ymin` et `ymax` sont des proportions de l’Axes.

## Documentation officielle

[Matplotlib — `pyplot.axvline`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.axvline.html)
