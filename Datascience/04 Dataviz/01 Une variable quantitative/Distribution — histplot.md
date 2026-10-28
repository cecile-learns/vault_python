# histplot

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Afficher la distribution d’une variable sous forme de classes. Peut aussi comparer des groupes avec `hue`.

## Syntaxe réutilisable

```python
ax = sns.histplot(data=df, x='valeur', bins=20, kde=True, color='steelblue')
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame contenant les variables. |
| `x` | Variable placée sur l’axe horizontal. |
| `y` | Permet un histogramme bivarié lorsqu’elle est fournie avec `x`. |
| `hue` | Sépare les observations en groupes colorés. |
| `bins` | Nombre de classes ou limites explicites des classes. |
| `binwidth` | Largeur souhaitée des classes. |
| `stat` | Hauteur des barres : `'count'`, `'frequency'`, `'probability'`, `'percent'` ou `'density'`. |
| `multiple` | Disposition des groupes : `'layer'`, `'dodge'`, `'stack'` ou `'fill'`. |
| `kde` | Ajoute une estimation de densité si `True`. |
| `element` | Aspect : `'bars'`, `'step'` ou `'poly'`. |
| `fill` | Remplit les barres ou zones si `True`. |
| `alpha` | Opacité entre 0 et 1. |
| `palette` | Palette utilisée quand `hue` est présent. |
| `ax` | Axes Matplotlib existant où tracer. |

## Fonction

Fonction de niveau Axes : elle renvoie l’objet `Axes` Matplotlib contenant l’histogramme.

## Vigilances & tips

- `bins` modifie fortement la lecture : tester plusieurs découpages cohérents avec les données.
- Avec `multiple='fill'`, chaque classe est normalisée à 1 : on compare des proportions, pas des effectifs.
- Éviter `kde=True` pour une variable discrète avec très peu de valeurs.

## Documentation officielle

[Seaborn — `histplot`](https://seaborn.pydata.org/generated/seaborn.histplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.


data=df peut aussi devenir conditionnelle data=df['age']>25
