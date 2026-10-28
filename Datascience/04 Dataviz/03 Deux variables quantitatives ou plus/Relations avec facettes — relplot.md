# relplot

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Créer des nuages de points ou des courbes répartis dans plusieurs facettes.

## Syntaxe réutilisable

```python
g = sns.relplot(data=df, x='x', y='y', hue='groupe', col='categorie', kind='scatter', height=4)
```
data = df[df['games']>60]
## Paramètres courants

| Paramètre       | Description                                      |     |
| --------------- | ------------------------------------------------ | --- |
| `data`          | DataFrame source.                                |     |
| `x / y`         | Variables des axes.                              |     |
| `hue`           | Variable encodée par couleur.                    |     |
| `size`          | Variable encodée par taille ou épaisseur.        |     |
| `style`         | Variable encodée par marqueur ou style de ligne. |     |
| **`row / col`** | Variables qui créent les facettes.               |     |
| `col_wrap`      | Nombre maximal de facettes par ligne.            |     |
| `kind`          | `'scatter'` ou `'line'`.                         |     |
| `height`        | Hauteur de chaque facette.                       |     |
| `aspect`        | Rapport largeur/hauteur de chaque facette.       |     |
| `facet_kws`     | Réglages supplémentaires de `FacetGrid`.         |     |
| `legend`        | Affiche ou règle la légende.                     |     |
| `palette`       | Palette associée à `hue`.                        |     |
| ci              | None pour ne pas avoir l'intervalle de confiance |     |

## Fonction

Fonction de niveau figure : elle renvoie un `FacetGrid` et appelle `scatterplot` ou `lineplot` selon `kind`.

## Vigilances & tips

- Ne pas utiliser `ax=`.
- Les paramètres propres au nuage ou à la courbe sont transmis à la fonction sous-jacente.
- Le résultat `g` permet notamment d’accéder à la figure avec `g.fig`.
data=df peut aussi devenir conditionnelle data=df['age']>25
## Documentation officielle

[Seaborn — `relplot`](https://seaborn.pydata.org/generated/seaborn.relplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
```Python

sns.relplot(data = df[df['games']>60], 
            x='games', y='pers_fouls',
           kind='line',
            ci=None
           )
plt.show()
```
![[Pasted image 20260917154102.png]]
