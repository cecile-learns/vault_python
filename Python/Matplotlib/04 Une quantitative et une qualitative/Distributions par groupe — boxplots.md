# Distributions par groupe — boxplots

## Pour faire quoi ?

Comparer la dispersion d’une mesure entre catégories.

## Script réutilisable

```python
import matplotlib.pyplot as plt

col_groupe = 'categorie'
col_valeur = 'montant'
df.boxplot(column=col_valeur, by=col_groupe)
plt.title(f'{col_valeur} par {col_groupe}')
plt.suptitle('')
plt.tight_layout()
plt.show()
```

## À adapter

- `df`, `col_groupe`, `col_valeur` : données, catégorie et mesure numérique.

## Vigilances & tips

`plt.suptitle('')` enlève le titre global automatique de Pandas. Les boîtes ne montrent pas les effectifs des groupes ni toute la forme des distributions.

Source de la séquence : AED Partie 2, §4 ; Liora, Box Plots. Les noms de variables et les libellés ont été généralisés.
