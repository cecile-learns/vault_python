# kdeplot

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Estimer une distribution continue par une courbe lissée, ou une distribution bivariée par niveaux colorés.

## Syntaxe réutilisable

```python
ax = sns.kdeplot(data=df, x='valeur', hue='groupe', fill=True, common_norm=False)
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x` | Variable quantitative principale. |
| `y` | Seconde variable quantitative pour une densité bivariée. |
| `hue` | Variable de regroupement par couleur. |
| `fill` | Remplit la surface sous la courbe ou entre les niveaux. |
| `bw_adjust` | Multiplie la largeur de lissage ; plus grand = plus lisse. |
| `cut` | Étend la courbe au-delà des valeurs observées en multiples de la bande passante. |
| `clip` | Limite le calcul à un intervalle. |
| `multiple` | Combine les groupes avec `'layer'`, `'stack'` ou `'fill'`. |
| `common_norm` | Normalise ensemble les groupes si `True`, séparément si `False`. |
| `levels` | Nombre ou liste des niveaux pour une densité bivariée. |
| `thresh` | Masque les zones sous un seuil de densité. |
| `cmap` | Palette continue pour une densité bivariée. |
| `alpha` | Opacité du remplissage. |
| `ax` | Axes Matplotlib existant où tracer. |
ne pas outlier shade =True également pour colorer l'air sous la courbe

## Fonction

Fonction de niveau Axes : elle renvoie l’`Axes` contenant l’estimation de densité.

## Vigilances & tips

- La courbe est une estimation : elle dépend du lissage `bw_adjust`.
- Une densité peut dépasser les bornes naturelles ; utiliser `cut=0` ou `clip=(min, max)` si nécessaire.
- Éviter une KDE pour des données discrètes ou un très petit échantillon.

## Documentation officielle

[Seaborn — `kdeplot`](https://seaborn.pydata.org/generated/seaborn.kdeplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.

```python
sns.kdeplot(df['age'],
            cut=0,
            shade=True
           
           
           ) ;

```
![[Pasted image 20260917143006.png]]
