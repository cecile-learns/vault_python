# light_palette

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Construire une palette séquentielle allant d’une teinte claire vers une couleur.

## Syntaxe réutilisable

```python
palette = sns.light_palette('seagreen', n_colors=6, reverse=False, as_cmap=False)
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `color` | Couleur de référence. |
| `n_colors` | Nombre de couleurs discrètes retournées. |
| `reverse` | Inverse l’ordre si `True`. |
| `as_cmap` | Renvoie une colormap continue si `True`. |
| `input` | Espace de définition de la couleur : `'rgb'`, `'hls'`, `'husl'` ou `'xkcd'`. |

## Fonction

Renvoie une liste de couleurs, ou une colormap Matplotlib avec `as_cmap=True`.

## Vigilances & tips

- Utiliser `as_cmap=True` pour une variable quantitative continue.
- Une palette claire convient à des valeurs allant de faible à forte.
- Vérifier le contraste lorsque la couleur la plus claire se rapproche du fond.

## Documentation officielle

[Seaborn — `light_palette`](https://seaborn.pydata.org/generated/seaborn.light_palette.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
