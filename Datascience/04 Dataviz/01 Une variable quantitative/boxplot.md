# boxplot

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
boites = plt.boxplot([valeurs_groupe_1, valeurs_groupe_2])
```

## Paramètres

Liste de séries numériques : une boîte par série.

## Fonction

Retourne un dictionnaire des éléments graphiques du boxplot.

## Vigilances & tips

Les positions commencent à 1 : `plt.xticks([1, 2], ["A", "B"])`. Nettoyer les valeurs non finies avant le tracé. Ne pas confondre avec `df.boxplot()` ou `sns.boxplot()`.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.



## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `tick_labels` | Libellés des boîtes, dans le même ordre que les séries. |
| `widths` | Largeur d’une boîte ou liste des largeurs. |
| `showmeans=True` | Affiche la moyenne. |
| `showfliers=False` | Masque les points situés au-delà des moustaches. |
| `patch_artist=True` | Crée des boîtes remplies afin de pouvoir régler leur couleur de fond. |
| `notch=True` | Dessine une encoche autour de la médiane. |

## Documentation officielle

[Matplotlib — `pyplot.boxplot`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.boxplot.html)

```python
df['Mois']= df.Month.apply(lambda x : x[3:])

l=list()
for i in df.Mois.unique():
    l.append(df[df['Mois'] == i]['Turnover'])
plt.boxplot(l)
plt.xticks(range(1,13),df.Mois.unique())

plt.show()
```
