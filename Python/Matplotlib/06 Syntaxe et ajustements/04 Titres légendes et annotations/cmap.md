# cmap

## Syntaxe

```python
nuage = plt.scatter(valeurs_x, valeurs_y,
                    c=valeurs_couleur, cmap='viridis')


plt.colorbar(nuage, label='Valeur représentée')
```

## Paramètres

- `valeurs_x`, `valeurs_y` : coordonnées de même longueur.
- `c=valeurs_couleur` : une valeur numérique par point, à représenter par une couleur.
- `cmap='viridis'` : palette utilisée pour convertir ces valeurs en couleurs.
- `nuage` : collection retournée par `scatter`, transmise à `colorbar` pour afficher la même échelle.
- `label` : texte de l’échelle de couleurs ; à remplacer par le nom et, si utile, l’unité de la variable.

## Fonction

`cmap` est un **paramètre**, pas une fonction autonome : il choisit une palette associant les valeurs à des couleurs. Il est également utilisé par d’autres fonctions, notamment `imshow`, `pcolormesh` et `contourf`.

## Mémo — choisir une palette

| Besoin | Valeurs possibles de `cmap` |
|---|---|
| Quantité de faible à élevée | `'viridis'`, `'plasma'`, `'cividis'` |
| Dégradé d’une couleur | `'Blues'`, `'Greens'` |
| Écarts autour d’une référence | `'coolwarm'`, `'RdBu'` |
| Catégories sans ordre | `'tab10'`, `'Set2'` |

Respecter les majuscules des noms. Pour inverser une palette intégrée, ajouter `_r` : `cmap='viridis_r'`.

## Vigilances & tips

- `c` fournit les valeurs ; `cmap` choisit la palette ; `colorbar` affiche leur correspondance.
- Avec `color='red'` ou `c=['red', 'blue', 'green']`, les couleurs sont déjà fixées : `cmap` n’intervient pas.
- Par défaut, l’échelle s’adapte aux valeurs du tracé. Deux graphiques peuvent donc attribuer des couleurs différentes à une même valeur ; comparer leurs échelles.
- Une palette divergente ne centre pas automatiquement l’échelle sur zéro : la normalisation doit aussi être adaptée à la référence choisie.
- Pour des catégories, conserver une correspondance explicite et stable entre catégories et couleurs ; des codes numériques ne deviennent pas automatiquement des catégories pour Matplotlib.

**S’utilise souvent avec :** `scatter()`, `colorbar()`.

## Documentation officielle

- [Matplotlib — guide visuel des palettes](https://matplotlib.org/stable/users/explain/colors/colormaps.html)
- [Matplotlib — utilisation de cmap dans scatter](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.scatter.html)

Source : paramètre rencontré dans ton exercice et échange sur `cmap='viridis'` ; documentation officielle Matplotlib.
