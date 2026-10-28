# axes

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
ax_zoom = plt.axes(
    [0.55, 0.4, 0.35, 0.35],
    facecolor='whitesmoke',
    xlim=(0, 10),
    ylim=(0, 100),
    xlabel='Temps',
    ylabel='Valeur',
    title='Vue agrandie'
)
```

## Paramètres

| Paramètre | Description |
|---|---|
| `[left, bottom, width, height]` | Position et dimensions de l’Axes, exprimées en fractions de la figure. Par exemple, `left=0.55` commence à 55 % de la largeur de la figure. |
| `facecolor` ou `fc` | Couleur de fond de la zone de tracé. Accepte notamment un nom (`'whitesmoke'`), un code abrégé (`'k'`) ou hexadécimal (`'#f0f0f0'`). |
| `frame_on` | Affiche (`True`) ou masque (`False`) le cadre et le fond de l’Axes. |
| `xlim` | Limites de l’axe horizontal sous la forme `(minimum, maximum)`. |
| `ylim` | Limites de l’axe vertical sous la forme `(minimum, maximum)`. |
| `xlabel` | Texte de l’étiquette de l’axe horizontal. |
| `ylabel` | Texte de l’étiquette de l’axe vertical. |
| `title` | Titre de l’Axes. |
| `xscale` | Échelle horizontale, généralement `'linear'` ou `'log'`. |
| `yscale` | Échelle verticale, généralement `'linear'` ou `'log'`. |
| `aspect` | Rapport d’échelle entre x et y. `'auto'` remplit l’espace disponible ; `'equal'` donne la même taille visuelle à une unité en x et en y. |
| `sharex` | Axes existant dont le nouvel Axes partage les limites, graduations et l’échelle horizontales. |
| `sharey` | Axes existant dont le nouvel Axes partage les limites, graduations et l’échelle verticales. |
| `projection` | Type de projection, par exemple `'polar'`. Sans valeur, la projection est rectangulaire. |
| `polar=True` | Raccourci équivalent à `projection='polar'`. |
| `axisbelow` | Place la grille et les graduations sous les données avec `True`, au-dessus avec `False`, ou selon un ordre intermédiaire avec `'line'`. |
| `xmargin`, `ymargin` | Marge automatique autour des données sur chaque axe. Par exemple, `0.05` correspond à une marge de 5 %. |

Tous ces réglages peuvent être passés lors de la création. Les méthodes `ax_zoom.set_xlim()`, `set_ylim()`, `set_xlabel()`, `set_ylabel()` et `set_title()` permettent aussi de les modifier ensuite.

## Exemples ciblés

### Changer uniquement la couleur de fond

```python
ax = plt.axes(facecolor='lightyellow')
```

`facecolor` colore l’intérieur de la zone de tracé. Il ne change pas le fond de toute la figure.

### Créer un Axes polaire

```python
ax_polaire = plt.axes(projection='polar')
```

## Fonction

Crée un objet `Axes`, c’est-à-dire la zone contenant un graphique, puis le rend courant pour les prochains appels `plt.…`.

## Vigilances & tips

- Les coordonnées du rectangle décrivent la figure, pas les données.
- `width` et `height` sont des proportions : ils doivent rester compatibles avec `left` et `bottom` pour que l’Axes tienne dans la figure.
- `facecolor` concerne l’Axes ; pour le fond complet de la figure, utiliser le paramètre `facecolor` de `plt.figure()`.
- Une échelle logarithmique exige des valeurs strictement positives sur l’axe concerné.
- `sharex` et `sharey` attendent un objet `Axes` existant, pas une chaîne de caractères.
- Pour une grille régulière de graphiques, `plt.subplots()` est généralement plus simple ; `plt.axes()` est utile pour un placement manuel, notamment un encart ou une vue agrandie.

**`plt.axes([gauche, bas, largeur, hauteur])`** permet de créer un nouvel espace graphique à l’intérieur de la figure. Les 4 valeurs sont des proportions de la figure entre 0 et 1 : position depuis la gauche, position depuis le bas, largeur et hauteur. Une fois cet axe créé, les tracés suivants s’affichent dedans jusqu’à ce qu’on change d’axe. Pour un zoom, on retrace donc les mêmes courbes dans cet espace puis on limite la zone avec `xlim/ylim` ou `axis`. À retenir aussi : **`plot()` relie simplement une suite de points `(x, y)`**, donc il peut servir à tracer un rectangle ou une autre forme, pas seulement une courbe mathématique.
## Documentation officielle

- [Matplotlib — `pyplot.axes`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.axes.html)
- [Matplotlib — paramètres de l’objet `Axes`](https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.html)

Source : Liora, cours Matplotlib ; documentation officielle Matplotlib.
