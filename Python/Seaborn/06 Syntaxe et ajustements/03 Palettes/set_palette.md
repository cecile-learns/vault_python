# set_palette

## Pour faire quoi ?

Définir le cycle global de couleurs utilisé par les graphiques suivants.

## Syntaxe réutilisable

```python
sns.set_palette('colorblind', n_colors=6, desat=None)
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `palette` | Nom de palette, séquence de couleurs ou objet palette. |
| `n_colors` | Nombre de couleurs dans le cycle. |
| `desat` | Proportion de saturation appliquée aux couleurs. |
| `color_codes` | Modifie aussi les codes abrégés Matplotlib si `True`. |

## Fonction

Modifie le cycle de couleurs global de Matplotlib à partir d’une palette Seaborn.

## Vigilances & tips

- Pour un seul graphique, préférer son paramètre `palette=`.
- Une palette qualitative distingue des catégories ; une palette séquentielle code un ordre ou une intensité.
- `'colorblind'` améliore souvent la distinction des catégories.

## Documentation officielle

[Seaborn — `set_palette`](https://seaborn.pydata.org/generated/seaborn.set_palette.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
