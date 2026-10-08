# Valeur agrégée par groupe — barres

## Pour faire quoi ?

Comparer une somme par catégorie, après agrégation.

## Script réutilisable

```python
import matplotlib.pyplot as plt

col_groupe = 'categorie'
col_valeur = 'montant'
resume = df.groupby(col_groupe)[col_valeur].sum().reset_index()
resume = resume.sort_values(col_valeur, ascending=True)
plt.barh(y=resume[col_groupe], width=resume[col_valeur],
         height=0.6, color='steelblue', edgecolor='black')
plt.show()
```

## À adapter

- `df`, `col_groupe`, `col_valeur`.
- Remplacer `.sum()` par `.mean()` si l’objectif est de comparer des moyennes.
- `height` : épaisseur des barres ; `width` : valeurs représentées.

## Vigilances & tips

Matplotlib trace les valeurs fournies ; il ne calcule pas les moyennes à ta place. Les catégories absentes du regroupement n’apparaissent pas.

Source de la séquence : Data Visualisation, §2.2–2.3 ; variante moyenne rencontrée dans l’analyse qualitative Liora. Les noms de variables et les libellés ont été généralisés.
