# Effectifs ou fréquences — barres

## Pour faire quoi ?

Comparer le nombre d’observations dans chaque catégorie.

## Script réutilisable

```python
import matplotlib.pyplot as plt

col_groupe = 'categorie'
effectifs = df[col_groupe].value_counts().sort_index()
effectifs.plot(kind='bar', figsize=(10, 4), edgecolor='black')
plt.title(f'Effectifs de {col_groupe}')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

#exemple du cours 
df=pd.read_csv('sales_data.csv')
display(df.head())
positions= range(0,len(df),3)

df.plot.bar(x='Month',y='Product1')

plt.xticks(positions, df['Month'].iloc[::3]);

```

## À adapter

- `df`, `col_groupe` : données et colonne catégorielle.
- Pour des pourcentages : remplacer le calcul par `effectifs = df[col_groupe].value_counts(normalize=True) * 100` et adapter le titre.

## Vigilances & tips

`value_counts()` exclut les valeurs manquantes par défaut. Le pourcentage porte donc sur les catégories renseignées. Le tracé utilise ici Pandas, puis Matplotlib pour sa présentation.

Source de la séquence : AED Partie 2, §3. Les noms de variables et les libellés ont été généralisés.

```python
#exemple de cours
plt.bar([1,3,5,7,9],[5,2,7,8,2],color='k', label='Exemple 1');
plt.bar([2,4,6,8,10],[8,6,2,5,6],color='r', label='Exemple 2');

plt.xlabel('Numéros')
plt.ylabel('Hauteur')

plt.legend()
plt.title('Mon graphique en barre')

plt.show()
                 
                 
```