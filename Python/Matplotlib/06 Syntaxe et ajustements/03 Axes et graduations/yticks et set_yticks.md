# yticks et set_yticks

## Syntaxe

```python
positions, textes = plt.yticks([0, 1, 2], ["A", "B", "C"])
# Sur un Axes existant, pour masquer les graduations :
ax.set_yticks([])
```

## Paramètres

Première liste : positions ; seconde : libellés correspondants. `[]` : aucune graduation.

## Fonction

Définit les graduations ; la forme pyplot retourne les positions et les étiquettes.

## Vigilances & tips

Autre usage rencontré : `plt.xticks(rotation=45)` incline les étiquettes sans changer leurs positions.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `ticks` | Positions des graduations. |
| `labels` | Textes correspondants ; leur nombre doit correspondre aux positions. |
| `minor=True` | Cible les graduations secondaires. |
| `rotation` | Inclinaison des textes en degrés. |
| `fontsize` | Taille des libellés. |
| `va` | Alignement vertical des libellés. |

## Documentation officielle

- [Matplotlib — `pyplot.yticks`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.yticks.html)
- [Matplotlib — `Axes.set_yticks`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_yticks.html)
