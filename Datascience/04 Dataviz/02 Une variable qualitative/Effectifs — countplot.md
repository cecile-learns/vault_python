# countplot

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Compter automatiquement les observations de chaque catégorie.

## Syntaxe réutilisable

```python
ax = sns.countplot(data=df, x='categorie', hue='groupe', order=ordre, stat='count')

sns.countplot(y = 'payment_method',data = df)

```

Attention si x='category' graphique vertical si y='category graphique horizontal

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x` | Catégorie placée horizontalement. |
| `y` | Alternative à `x` pour des barres horizontales. |
| `hue` | Seconde variable catégorielle. |
| `order` | Ordre explicite des catégories. |
| `hue_order` | Ordre des modalités de `hue`. |
| `stat` | Valeur affichée : `'count'`, `'percent'`, `'proportion'` ou `'probability'`. |
| `palette` | Palette des groupes de `hue`. |
| `color` | Couleur unique des barres. |
| `saturation` | Saturation des couleurs, entre 0 et 1. |
| `width` | Largeur des barres. |
| `gap` | Espace ajouté entre les barres. |
| `legend` | Affiche ou règle la légende. |
| `ax` | Axes Matplotlib existant. |

## Fonction

Fonction de niveau Axes : elle effectue le comptage et renvoie l’`Axes`.

## Vigilances & tips

- Ne pas lui fournir une colonne déjà agrégée : `countplot` compte les lignes.
- Pour tracer une mesure calculée par catégorie, utiliser `barplot`.
- Un `order` explicite évite un classement inattendu.

## Documentation officielle

[Seaborn — `countplot`](https://seaborn.pydata.org/generated/seaborn.countplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
