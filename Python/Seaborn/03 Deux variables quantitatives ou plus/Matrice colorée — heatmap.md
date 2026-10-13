# heatmap

## Pour faire quoi ?

Représenter une matrice numérique par des couleurs, par exemple une matrice de corrélation.

## Syntaxe réutilisable

```python
matrice=df.select_dtypes(include='number').corr()
ax = sns.heatmap(matrice, annot=True, fmt='.2f', cmap='coolwarm', center=0, linewidths=0.5)

sns.heatmap(cor, annot = True, ax = ax, cmap = "coolwarm")


cor = df_taste.iloc[:, 1:11].corr()

plt.figure(figsize = (10, 6))
sns.heatmap(cor, annot = True)
```

## Paramètres courants

| Paramètre                   | Description                                                        |
| --------------------------- | ------------------------------------------------------------------ |
| `data`                      | Matrice 2D ou DataFrame rectangulaire.                             |
| `vmin / vmax`               | Bornes de l’échelle de couleurs.                                   |
| `cmap`                      | Palette continue ou divergente.                                    |
| `center`                    | Valeur centrale d’une palette divergente.                          |
| `robust`                    | Calcule des bornes robustes avec des quantiles si `True`.          |
| `annot`                     | Affiche une valeur ou une matrice d’annotations dans les cellules. |
| `fmt`                       | Format des annotations, par exemple `'.2f'`.                       |
| `annot_kws`                 | Style du texte des annotations.                                    |
| `linewidths`                | Épaisseur des séparations entre cellules.                          |
| `linecolor`                 | Couleur des séparations.                                           |
| `cbar`                      | Affiche la barre de couleurs.                                      |
| `cbar_kws`                  | Réglages de la barre de couleurs.                                  |
| `square`                    | Rend les cellules carrées si `True`.                               |
| `mask`                      | Masque booléen des cellules à cacher.                              |
| `xticklabels / yticklabels` | Affichage ou espacement des libellés.                              |
| `ax`                        | Axes Matplotlib existant.                                          |

## Fonction

Fonction de niveau Axes : elle renvoie l’`Axes` contenant la matrice colorée.

## Vigilances & tips

- Pour une corrélation, `center=0` et une palette divergente rendent le signe plus lisible.
- Les annotations deviennent illisibles sur une grande matrice.
- Un masque triangulaire évite de répéter une matrice symétrique.

## Documentation officielle

[Seaborn — `heatmap`](https://seaborn.pydata.org/generated/seaborn.heatmap.html)

| Besoin                     | `cmap` utiles                     | Quand                                           |
| -------------------------- | --------------------------------- | ----------------------------------------------- |
| Valeurs faibles → fortes   | `'viridis'`, `'Blues'`            | variable quantitative avec progression          |
| Valeurs autour d'un centre | `'coolwarm'`, `'RdBu'`, `'vlag'`  | valeurs négatives ↔ positives, **corrélations** |
| Intensité simple           | `'Greens'`, `'Oranges'`, `'Reds'` | comptages, intensités positives                 |
| Ordre inversé              | `'viridis_r'`, `'Blues_r'`        | suffixe `_r` inverse la palette                 |