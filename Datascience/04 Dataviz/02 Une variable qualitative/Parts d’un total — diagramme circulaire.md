# Parts d’un total — diagramme circulaire

> [!info] Bibliothèque
> Matplotlib

## Pour faire quoi ?

Représenter la contribution de chaque catégorie à une somme.

## Script réutilisable

```python
import matplotlib.pyplot as plt

col_groupe = 'categorie'
col_valeur = 'montant'
totaux = df.groupby(col_groupe)[col_valeur].sum().reset_index()
plt.pie(totaux[col_valeur], labels=totaux[col_groupe],
        autopct='%1.1f%%', startangle=90)
plt.show()

#Exemple cours
plt.figure(figsize=(7, 7))

plt.pie(df.head(6).Turnover,
       labels=['Janv', 'Fev', 'Mars', 'Avril', 'Mai', 'Juin'],
       colors=['red', 'orange','yellow', 'green', 'blue', 'purple'],
       explode=[0,0,0,0.2,0,0],
       autopct=lambda pourcentage: f'{pourcentage:.2f} %',
       pctdistance=0.7,
       labeldistance=1.2,
        shadow=True  
       
       )

plt.legend();
```

## À adapter

- `df`, `col_groupe`, `col_valeur` : données, catégories et valeurs à additionner.
- `autopct` : pourcentage à une décimale ; `startangle=90` : départ en haut.

## Vigilances & tips

Les parts doivent être non négatives et leur total strictement positif. Ici, les parts représentent des sommes, pas des effectifs : la colonne numérique sert de poids aux catégories. Garder peu de catégories.

Source de la séquence : Data Visualisation, §2.2 ; affichage explicite ajouté pour usage hors notebook. Les noms de variables et les libellés ont été généralisés.


Les diagrammes circulaires s'affichent mieux si la figure présente un format agrandi et **carré**.  
Lorsque l'on utilise une fonction de **pyplot**, une figure se crée automatiquement, au format standard par défaut.  
La figure peut être crée aux dimensions **(h,l)** grâce à la commande **plt.figure( figsize= (h, l) )**.
