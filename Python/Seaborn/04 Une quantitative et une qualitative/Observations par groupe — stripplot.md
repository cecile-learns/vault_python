# stripplot

## Pour faire quoi ?

Afficher chaque observation d’une variable quantitative par catégorie, avec un léger décalage pour limiter les superpositions.

## Syntaxe réutilisable

```python
ax = sns.stripplot(data=df, x='categorie', y='valeur', hue='groupe', jitter=True, alpha=0.6, size=4)
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `data` | DataFrame source. |
| `x / y` | Catégorie et mesure. |
| `hue` | Sous-groupes. |
| `order / hue_order` | Ordre des groupes. |
| `jitter` | Décalage aléatoire : booléen ou amplitude. |
| `dodge` | Sépare les niveaux de `hue` si `True`. |
| `size` | Taille des marqueurs. |
| `marker` | Forme des marqueurs. |
| `linewidth` | Épaisseur du contour. |
| `edgecolor` | Couleur du contour. |
| `alpha` | Opacité. |
| `palette / color` | Couleurs. |
| `legend` | Réglage de la légende. |
| `ax` | Axes Matplotlib existant. |

## Fonction

Fonction de niveau Axes : elle affiche les observations avec un décalage aléatoire facultatif et renvoie l’`Axes`.

## Vigilances & tips

- Le nom correct est `stripplot`; `striplot` rencontré dans une ressource est une faute de frappe.
- Le jitter change seulement la position visuelle, pas les données.
- Avec beaucoup de points, réduire `size` et `alpha`, ou utiliser un résumé de distribution.

## Documentation officielle

[Seaborn — `stripplot`](https://seaborn.pydata.org/generated/seaborn.stripplot.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
