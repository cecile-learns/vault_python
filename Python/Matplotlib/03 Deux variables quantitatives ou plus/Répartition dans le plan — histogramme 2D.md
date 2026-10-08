# Répartition dans le plan — histogramme 2D

## Pour faire quoi ?

Regrouper les couples de valeurs dans des cellules colorées.

## Script réutilisable

```python
import matplotlib.pyplot as plt

col_x = 'longitude'
col_y = 'latitude'
fig, ax = plt.subplots(figsize=(10, 6))
histogramme = ax.hist2d(df[col_x], df[col_y], bins=50, density=True)
fig.colorbar(histogramme[3])
ax.set_xlabel(col_x)
ax.set_ylabel(col_y)
ax.set_title('Distribution jointe')
plt.show()
```

## À adapter

- `df`, `col_x`, `col_y` : colonnes numériques sans valeurs non finies.
- `bins` : nombre d’intervalles par dimension ; `density` : normalisation en densité.

## Vigilances & tips

`histogramme[3]` est l’objet coloré associé à l’échelle. La couleur représente une densité si `density=True`, un effectif si `False`.

Source de la séquence : AED Partie 2, §6. Les noms de variables et les libellés ont été généralisés.
