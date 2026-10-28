# axis

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
limites = plt.axis([xmin, xmax, ymin, ymax])

plt.axis(xmin=80, xmax=180, ymin=1, ymax=5)
```

## Paramètres

Bornes minimale et maximale pour x, puis pour y.

## Fonction

Fixe les quatre limites et les retourne.

## Vigilances & tips

Limiter l’affichage ne filtre pas les données sous-jacentes.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

`plt.axis()` accepte également des options textuelles courantes :

| Option | Effet |
|---|---|
| `'off'` / `'on'` | Masque ou affiche les axes. |
| `'equal'` | Utilise la même échelle visuelle pour une unité en x et en y. |
| `'scaled'` | Conserve une échelle égale en ajustant les dimensions de la zone de tracé. |
| `'tight'` | Ajuste les limites au plus près des données. |
| `'auto'` | Autorise Matplotlib à choisir les limites et le rapport d’aspect. |

## Documentation officielle

[Matplotlib — `pyplot.axis`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.axis.html)
