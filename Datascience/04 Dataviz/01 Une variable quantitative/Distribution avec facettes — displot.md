# displot

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Comparer des distributions et créer facilement une ligne ou une colonne de sous-graphiques.

## Syntaxe réutilisable

```python
g = sns.displot(data=df, x='valeur', hue='groupe', col='categorie', kind='hist', height=4, aspect=1.2)
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x` | Variable à distribuer. |
| `y` | Seconde variable pour un tracé bivarié. |
| `hue` | Groupes distingués par couleur. |
| `row` | Variable qui crée une ligne de facettes par modalité. |
| `col` | Variable qui crée une colonne de facettes par modalité. |
| `col_wrap` | Nombre maximal de facettes par ligne. |
| `kind` | Type de tracé : `'hist'`, `'kde'` ou `'ecdf'`. |
| `height` | Hauteur en pouces de chaque facette. |
| `aspect` | Rapport largeur/hauteur de chaque facette. |
| `facet_kws` | Dictionnaire transmis à `FacetGrid`. |
| `legend` | Affiche la légende si `True`. |
| `bins / kde / multiple` | Paramètres transmis au tracé sous-jacent selon `kind`. |
Paramètre manquant : kde= true pour avoir la courbe de densité
## Fonction

Fonction de niveau figure : elle crée sa propre figure et renvoie un `FacetGrid`.

## Vigilances & tips

- Ne pas lui passer `ax=` : elle gère elle-même la figure.
- La largeur d’une facette vaut `height * aspect`.
- Les paramètres spécifiques dépendent de `kind` ; consulter aussi la documentation de `histplot` ou `kdeplot`.

data=df peut aussi devenir conditionnelle data=df['age']>25

## Documentation officielle

[Seaborn — `displot`](https://seaborn.pydata.org/generated/seaborn.displot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.


```python
sns.displot(df.age, bins=10,
           kde=True,
            rug=True,
           color='red'           );

```

![[Pasted image 20260917143111.png]]
