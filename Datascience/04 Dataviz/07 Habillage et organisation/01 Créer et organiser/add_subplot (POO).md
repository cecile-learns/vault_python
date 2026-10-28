# add_subplot

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
ax = fig.add_subplot(grille[0, 1])
```

## Paramètres

`fig` : Figure existante ; `grille` : résultat de `fig.add_gridspec()` ; `[0, 1]` : première ligne, deuxième colonne.

## Fonction

Crée et retourne l’Axes occupant cette cellule.

Source : Data Visualisation, partie 4.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `projection='polar'` | Crée un Axes en coordonnées polaires. |
| `sharex=ax_reference` | Partage l’axe x avec un Axes existant. |
| `sharey=ax_reference` | Partage l’axe y avec un Axes existant. |
| `facecolor='whitesmoke'` | Définit la couleur de fond de l’Axes créé. |

## Documentation officielle

[Matplotlib — `Figure.add_subplot`](https://matplotlib.org/stable/api/_as_gen/matplotlib.figure.Figure.add_subplot.html)
