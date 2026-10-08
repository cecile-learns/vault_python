# plot

## Syntaxe

```python
courbes = plt.plot(valeurs_x, valeurs_y, color="blue", linestyle="--",
                   linewidth=1.5, marker="o", label="Mesure")
# Variante sur un Axes existant :
# courbes = ax.plot(valeurs_x, valeurs_y)
```

## Paramètres

`valeurs_x`, `valeurs_y` : coordonnées de même longueur. `color` : couleur ; `linestyle` : tirets ; `linewidth` : épaisseur ; `marker` : symbole ; `label` : texte pour la légende.

## Fonction

Relie les points dans l’ordre fourni et retourne une liste de lignes.

## Vigilances & tips

Sans x, les abscisses sont les positions 0, 1, 2… Plusieurs appels superposent les courbes. Forme compacte rencontrée : `plt.plot(x1, y1, "r--", x2, y2, "b-")`. Elle combine couleur et style.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.


## Mémo — styles, couleurs et marqueurs

### Styles de ligne — `linestyle`

| Code | Description |
|---|---|
| `'-'` | Ligne continue |
| `'--'` | Tirets |
| `':'` | Pointillés |
| `'-.'` | Alternance tirets et points |

### Couleurs abrégées — `color`

| Code | Couleur |
|---|---|
| `'b'` | Bleu |
| `'r'` | Rouge |
| `'c'` | Cyan |
| `'g'` | Vert |
| `'y'` | Jaune |
| `'k'` | Noir |
| `'w'` | Blanc |
| `'m'` | Magenta |

Ce sont les huit codes abrégés, pas toutes les couleurs disponibles. Les noms de couleurs et les codes hexadécimaux sont également utilisables.

### Marqueurs courants — `marker`

| Code | Description |
|---|---|
| `'.'` | Point |
| `','` | Pixel |
| `'o'` | Cercle |
| `'x'` | Croix |
| `'+'` | Plus |

### Combiner les trois

```python
courbes = plt.plot(valeurs_x, valeurs_y,
                   color='r', linestyle='--', marker='o')

# Écriture compacte équivalente, à utiliser à la place :
# courbes = plt.plot(valeurs_x, valeurs_y, 'r--o')
```

Ici : courbe rouge, en tirets, avec des cercles aux points. `valeurs_x` et `valeurs_y` sont les coordonnées de même longueur ; `courbes` contient les lignes retournées.

### Pour trouver les autres marqueurs

[Documentation officielle Matplotlib — marqueurs disponibles](https://matplotlib.org/stable/api/markers_api.html)

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `alpha` | Opacité de la courbe, entre `0` et `1`. |
| `markersize` ou `ms` | Taille des marqueurs. |
| `markerfacecolor` ou `mfc` | Couleur intérieure des marqueurs. |
| `markeredgecolor` ou `mec` | Couleur du contour des marqueurs. |
| `zorder` | Ordre de superposition ; une valeur élevée place la courbe au-dessus. |

## Documentation officielle de la fonction

[Matplotlib — `pyplot.plot`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html)
