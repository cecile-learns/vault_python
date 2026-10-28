# set_xticklabels

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
textes = ax.set_xticklabels(["A", "B", "C"])
```

## Paramètres

`ax` : graphique existant ; liste : un texte pour chaque graduation fixée.

## Fonction

Remplace les étiquettes des graduations x.

## Vigilances & tips

Fixer d’abord les positions avec `ax.set_xticks([0, 1, 2])`. Sinon les étiquettes peuvent se décaler quand les graduations changent.

Source : Data Visualisation, partie 4.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `labels` | Séquence de textes, correspondant aux positions déjà fixées. |
| `minor=True` | Cible les graduations secondaires. |
| `rotation` | Incline les libellés en degrés. |
| `fontsize` | Taille du texte. |
| `ha` | Alignement horizontal : `'left'`, `'center'` ou `'right'`. |

## Documentation officielle

[Matplotlib — `Axes.set_xticklabels`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.set_xticklabels.html)
