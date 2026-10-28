# subplots

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
fig, ax = plt.subplots(figsize=(8, 5))
# Autre configuration : plusieurs graphiques
fig, axes = plt.subplots(2, 2, figsize=(10, 8), sharex=True, sharey=True)
```

## Paramètres

`2, 2` : deux lignes et deux colonnes. 
`sharex`, `sharey` : axes partagés. 
`figsize` : dimensions de la figure entière en pouces.

## Fonction

Retourne une Figure et un Axes, ou un tableau d’Axes selon la grille.

## Vigilances & tips

Un graphique : `ax`. Une seule ligne ou colonne : `axes[i]`. Plusieurs lignes et colonnes : `axes[i, j]` ou boucle sur `axes.flatten()`. `flatten()` relève de NumPy ; ne pas l’appeler sur un Axes seul.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

Avec **Matplotlib**, `plt.subplot(...)` permet de construire une même figure contenant plusieurs graphiques de **types différents** : par exemple un barplot, un nuage de points, une courbe et un histogramme. Chaque emplacement de la grille est sélectionné puis rempli indépendamment. À l’inverse, avec **Pandas**, `subplots=True` dans `df.plot(...)` sépare plusieurs variables en plusieurs sous-graphiques, mais ces graphiques proviennent du **même type de méthode graphique**. Par exemple, `df.plot(..., subplots=True, layout=(2,2))` trace plusieurs séries séparément dans une grille, mais ne permet pas à lui seul d’associer automatiquement une colonne à un barplot, une autre à un scatter, une autre à un histogramme, etc. Pour mélanger plusieurs types de graphiques dans une même figure, `plt.subplot(...)` est donc plus adapté.


```python
plt.figure(figsize=(10,10))

plt.subplot(2,2,1)
plt.bar(range(len(df)), df.Product1, label = "Produit1")
plt.legend()

plt.subplot(2,2,2)
plt.scatter(df.Product1, df.Product2, label="Produit2")
plt.legend()

plt.subplot(2,2,3)
plt.plot(df.Returns, label="Retours")
plt.legend()

plt.subplot(2,2,4)
plt.hist(df.Turnover, label="Chiffre d'affaires")
plt.legend()
```
![[Pasted image 20260917110613.png]]
```python
df.plot(y = ['Product1', 'Product2', 'Returns', 'Turnover'], subplots=True, layout= (2,2),
        style = ['b--', 'm:p', 'g-.', 'c-d'], figsize=(7,7));
```

![[Pasted image 20260917110649.png|573]]


    

## Paramètres courants complémentaires de `subplots`

| Paramètre | Effet |
|---|---|
| `nrows`, `ncols` | Nombre de lignes et de colonnes d’Axes. |
| `squeeze=False` | Retourne toujours un tableau d’Axes à deux dimensions, même pour une seule ligne ou colonne. |
| `width_ratios` | Largeurs relatives des colonnes. |
| `height_ratios` | Hauteurs relatives des lignes. |
| `layout='constrained'` | Ajuste la disposition pour limiter les chevauchements. |
| `subplot_kw={...}` | Paramètres transmis à chaque Axes, par exemple `{'projection': 'polar'}`. |

Dans `plt.subplot(nrows, ncols, index)`, `index` commence à `1` et sélectionne la case courante.

## Documentation officielle

- [Matplotlib — `pyplot.subplots`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.subplots.html)
- [Matplotlib — `pyplot.subplot`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.subplot.html)
