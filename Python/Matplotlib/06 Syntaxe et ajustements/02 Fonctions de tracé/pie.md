# pie

## Syntaxe

```python
decalage_parts = [0, 0.1, 0, 0]
couleurs_parts = ['steelblue', 'orange', 'seagreen', 'tomato']

secteurs, textes, pourcentages = plt.pie(
    valeurs,
    labels=categories,
    explode=decalage_parts,
    colors=couleurs_parts,
    labeldistance=1.1,
    autopct=lambda pourcentage: f'{pourcentage:.1f} %',
    pctdistance=0.7,
    shadow=True,
    startangle=90
)
```

## Paramètres

| Paramètre | Description |
|---|---|
| `valeurs` | Poids non négatifs utilisés pour calculer la taille des parts. Le total doit être positif. |
| `labels` | Séquence contenant le nom de chaque part ; elle doit correspondre à `valeurs`. |
| `explode` | Séquence de même longueur que `valeurs`. Chaque nombre indique la fraction de rayon qui éloigne la part du centre : `0` ne la décale pas, `0.1` la décale de 10 % du rayon. |
| `colors` | Séquence des couleurs appliquées aux parts, dans leur ordre. Les noms, codes abrégés et codes hexadécimaux sont acceptés. |
| `labeldistance` | Distance entre le centre et les étiquettes, exprimée en fraction du rayon. Une valeur supérieure à `1` place les étiquettes à l’extérieur du cercle. |
| `autopct` | Texte affiché dans les parts. Accepte une chaîne de format comme `'%1.1f%%'` ou une fonction recevant le pourcentage calculé et renvoyant le texte à afficher. |
| `pctdistance` | Distance entre le centre et les pourcentages créés par `autopct`, en fraction du rayon. `1` les place sur le bord du cercle ; une valeur inférieure les rapproche du centre. |
| `shadow=True` | Ajoute une ombre sous le camembert. La valeur par défaut est `False`. |
| `startangle` | Rotation du départ en degrés, dans le sens inverse des aiguilles d’une montre à partir de l’axe horizontal. `90` place le départ en haut. |

### Écriture simple de `autopct`

```python
autopct='%1.1f%%'
```

Affiche un pourcentage avec une décimale, par exemple `27.5%`. Le premier `%` introduit le format numérique et `%%` affiche le symbole `%`.

### Écriture de `autopct` avec une fonction lambda

```python
autopct=lambda pourcentage: f'{pourcentage:.1f} %'
```

Matplotlib appelle cette fonction pour chaque part en lui transmettant son pourcentage. La fonction renvoie le texte à afficher. Cette forme permet notamment de modifier le format ou de masquer certaines valeurs.

## Fonction

Trace les parts d’un total. Avec `autopct`, retourne les secteurs, les étiquettes et les textes des pourcentages.

## Vigilances & tips

- `explode`, `colors` et `labels` suivent l’ordre de `valeurs` ; vérifier leur correspondance.
- Sans `autopct`, `plt.pie()` renvoie seulement les secteurs et les textes des étiquettes.
- Le comportement courant normalise les valeurs pour former un total égal à 1.
- Les diagrammes circulaires s’affichent mieux dans une figure carrée, par exemple `plt.figure(figsize=(6, 6))`.
- Trop de parts ou des valeurs proches rendent les comparaisons difficiles ; un diagramme en barres est alors souvent plus lisible.

## Documentation officielle

[Matplotlib — paramètres de `pyplot.pie`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.pie.html)

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation ; documentation officielle Matplotlib.
