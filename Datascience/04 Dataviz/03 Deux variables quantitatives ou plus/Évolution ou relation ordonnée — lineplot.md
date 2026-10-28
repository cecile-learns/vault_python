# lineplot

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Relier des valeurs selon un ordre, par exemple le temps, une vitesse ou une distance.

## Syntaxe réutilisable

```python
ax = sns.lineplot(data=df, x='abscisse', y='valeur', hue='groupe', marker='o', errorbar=None)
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x` | Variable qui ordonne les points. |
| `y` | Mesure représentée. |
| `hue` | Groupes distingués par couleur. |
| `style` | Groupes distingués par style de ligne ou marqueur. |
| `size` | Variable encodée par épaisseur de ligne. |
| `estimator` | Agrégation appliquée lorsque plusieurs observations partagent un même `x`; moyenne par défaut. |
| `errorbar` | Incertitude : `'ci'`, `'pi'`, `'se'`, `'sd'`, tuple ou `None`. |
| `sort` | Trie les points selon `x` avant de les relier. |
| `units` | Identifiant de séries individuelles, généralement avec `estimator=None`. |
| `markers` | Active ou choisit les marqueurs par groupe. |
| `dashes` | Active ou définit les styles de ligne. |
| `palette` | Palette des groupes. |
| `legend` | Réglage de la légende. |
| `ax` | Axes Matplotlib existant. |

## Fonction

Fonction de niveau Axes : elle peut agréger les répétitions et renvoie l’`Axes`.

## Vigilances & tips

- Par défaut, des observations répétées au même `x` sont moyennées et une incertitude est calculée.
- Utiliser `estimator=None` pour conserver les valeurs sans agrégation.
- Vérifier l’ordre de `x`, surtout s’il est stocké comme texte.

## Documentation officielle

[Seaborn — `lineplot`](https://seaborn.pydata.org/generated/seaborn.lineplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
