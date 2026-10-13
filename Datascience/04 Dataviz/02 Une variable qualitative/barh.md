# barh

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
barres = plt.barh(y=categories, width=valeurs, height=0.6)
```

## Paramètres

`y` : catégories ou positions ; `width` : valeurs ; `height` : épaisseur des barres.

## Fonction

Trace des barres horizontales.

## Vigilances & tips

Pratique pour des libellés longs. Inversion à retenir : les valeurs vont dans `width`, pas dans `height`.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `left` | Position de départ des barres sur l’axe x ; permet notamment l’empilement horizontal. |
| `align` | Aligne la catégorie au centre (`'center'`) ou sur le bord (`'edge'`). |
| `color`, `edgecolor` | Couleurs du remplissage et du contour. |
| `linewidth` | Épaisseur du contour. |
| `xerr`, `ecolor`, `capsize` | Valeurs, couleur et extrémités des barres d’erreur horizontales. |
| `label` | Texte utilisé par la légende. |

## Documentation officielle

[Matplotlib — `pyplot.barh`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.barh.html)
