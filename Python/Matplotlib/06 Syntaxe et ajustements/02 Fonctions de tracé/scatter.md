# scatter

## Syntaxe

```python
nuage = plt.scatter(valeurs_x, valeurs_y, s=40, c="blue", alpha=0.5, marker="o")
# Variante rencontrée :
# nuage = ax.scatter(valeurs_x, valeurs_y)

df.plot.scatter(x='Product1', y='Product2', c = 'Returns', s=30, cmap = 'viridis');
```

## Paramètres

x et y : couples de coordonnées ; 
`s` : aire des points en points typographiques au carré ; 
`c` : couleur unique, couleurs par point ou valeurs numériques ; `alpha` : opacité ; `marker` : symbole.

## Fonction

Trace des points non reliés et retourne leur collection.

## Vigilances & tips

Pour Matplotlib, utiliser `s`, pas `size`. Une liste de tailles ou de couleurs doit correspondre aux points. `c` numérique encode une valeur par la couleur.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Mémo — styles, couleurs et marqueurs

### Aspect des points

`scatter` ne relie pas les points. Les styles de ligne concernent ici les **contours des marqueurs**.

| Écriture | Effet |
|---|---|
| `s=60` | Taille en points typographiques au carré |
| `alpha=0.5` | Semi-transparent ; `0` invisible, `1` opaque |
| `edgecolors='k'` | Contours noirs |
| `edgecolors='none'` | Aucun contour |
| `facecolors='none', edgecolors='b'` | Marqueurs creux à contour bleu |
| `linewidths=1.5` | Épaisseur du contour en points |

### Styles de contour — `linestyles`

| Code | Description |
|---|---|
| `'-'` | Continu |
| `'--'` | Tirets |
| `':'` | Pointillés |
| `'-.'` | Tirets et points |

Exemple de paramètre : `linestyles='--'`. L’effet se voit surtout sur de grands marqueurs avec contour visible.

### Couleurs abrégées — `color` ou `c`

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

Les noms (`'orange'`) et codes hexadécimaux (`'#367d7d'`) fonctionnent aussi.

- Couleur commune : `color='r'`.
- Une couleur par point : `c=['r', 'g', 'b']` pour trois points.
- Valeurs numériques à convertir en couleurs : `c=valeurs_couleur, cmap='viridis'` ; `valeurs_couleur` contient une valeur par point, `cmap` choisit la palette.

Utiliser `color` ou `c`, pas les deux ensemble. Pour des cercles creux, omettre ces deux paramètres et utiliser `facecolors='none'` avec `edgecolors`.

### Marqueurs courants — `marker`

| Code | Description |
|---|---|
| `'.'` | Point |
| `','` | Pixel (nom du symbole ; taille pilotée par `s` dans scatter) |
| `'o'` | Cercle |
| `'x'` | Croix |
| `'+'` | Plus |
| `'s'` | Carré |
| `'^'` | Triangle vers le haut |
| `'*'` | Étoile |

Avec `'x'` et `'+'`, `edgecolors` est ignoré : régler la couleur avec `color` ou `c`.

### Exemple combiné

```python
nuage = plt.scatter(valeurs_x, valeurs_y,
                    s=80, marker='o', color='r', alpha=0.7,
                    edgecolors='k', linewidths=1)
```

Cercles rouges semi-transparents à contours noirs. `valeurs_x` et `valeurs_y` contiennent les coordonnées de même longueur ; `nuage` contient la collection retournée.

### Documentation officielle

- [Matplotlib — paramètres de scatter](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.scatter.html)
- [Matplotlib — tous les marqueurs](https://matplotlib.org/stable/api/markers_api.html)
- [Matplotlib — définir les couleurs](https://matplotlib.org/stable/users/explain/colors/colors.html)

