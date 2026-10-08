# Distributions par groupe — histogrammes séparés

## Pour faire quoi ?

Afficher un histogramme par catégorie sur une grille comparable.

## Script réutilisable

```python
import matplotlib.pyplot as plt

import numpy as np

col_groupe = 'categorie'
col_valeur = 'montant'
groupes = df[col_groupe].dropna().unique()
n_colonnes = 2
n_lignes = (len(groupes) + n_colonnes - 1) // n_colonnes
bornes = np.linspace(df[col_valeur].min(), df[col_valeur].max(), 21)
fig, axes = plt.subplots(n_lignes, n_colonnes, figsize=(12, 4 * n_lignes),
                         sharex=True, sharey=True)
for ax, groupe in zip(axes.flatten(), groupes):
    sous_groupe = df[df[col_groupe] == groupe]
    ax.hist(sous_groupe[col_valeur], bins=bornes, color='steelblue', edgecolor='white')
    ax.set_title(str(groupe))
    ax.set_xlabel(col_valeur)
    ax.set_ylabel('Effectif')
plt.suptitle(f'Distribution de {col_valeur} par {col_groupe}')
plt.tight_layout()
plt.show()
```

## À adapter

- `df`, `col_groupe`, `col_valeur`.
- `n_colonnes` : conserver au moins 2 pour que `axes.flatten()` reste applicable.
- `21` bornes donnent 20 intervalles communs.

## Vigilances & tips

Prévoir au moins un groupe et une colonne numérique non constante, avec des valeurs finies. Une case peut rester vide si le nombre de groupes est impair. Des effectifs de groupe différents influencent les hauteurs. Adaptations du script source : grille dimensionnée au nombre de groupes et bornes communes pour éviter de comparer des intervalles différents.

Source de la séquence : Data Visualisation, §2.6 ; boucle sur les Axes également rencontrée dans Anscombe. Les noms de variables et les libellés ont été généralisés.
