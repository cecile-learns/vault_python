# Distribution — histogramme

## Pour faire quoi ?

Afficher la distribution d’une colonne numérique, avec sa moyenne et sa médiane.

## Script réutilisable

```python
import matplotlib.pyplot as plt

col_valeur = 'montant'
plt.figure(figsize=(8, 4))

df[col_valeur].hist(bins=30, density=True, edgecolor='black')

plt.axvline(df[col_valeur].mean(), color='red', linestyle='--', label='Moyenne')

plt.axvline(df[col_valeur].median(), color='green', linestyle='--', 
label='Médiane')
plt.xlabel(col_valeur)
plt.ylabel('Densité')
plt.title(f'Distribution de {col_valeur}')
plt.legend()
plt.show()



#Méthode avec pandas
df.plot.hist(y=['Product1', 'Product2'], 
bins = 7, 
rwidth = 0.8 , 
color= ['#0c4c83', '#830c4c'], 
alpha=0.5);

df.plot.hist(y=['Product1', 'Product2'], 
bins = 7, 
subplots=True, 
rwidth = 0.8 , 
color= ['#0c4c83', '#830c4c'], 
alpha=0.5);

df.hist(column=['Product1', 'Product2'], 
bins = 7, 
rwidth = 0.8 , 
color= ['#0c4c83']);
```

## À adapter

- `df` : ton DataFrame déjà chargé ; `col_valeur` : colonne numérique.
- `bins` : nombre d’intervalles ; `figsize` : largeur et hauteur en pouces.

## Vigilances & tips

`density=True` normalise l’aire totale à 1, pas la somme des hauteurs. Pour des effectifs, passer à `density=False` et remplacer le label Y par « Effectif ». Les repères moyenne/médiane ne prouvent pas la normalité.

Source de la séquence : AED Partie 2, §2. Les noms de variables et les libellés ont été généralisés.
