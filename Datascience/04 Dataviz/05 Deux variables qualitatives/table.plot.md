
> [!info] Bibliothèque
> Matplotlib


# 

## Pour faire quoi ?

Comparer deux variables qualitatives

## Script réutilisable

```python
import matplotlib.pyplot as plt

table = pd.crosstab(df['Pclass'], df['Survived'],normalize='index')
display(table)

table.plot(kind='bar')

plt.xlabel('Classe')
plt.ylabel('Proportion')
plt.title('Taux de survie selon la classe')
plt.legend(['Décédé', 'Survécu'])
plt.xticks(rotation=0)

plt.show()



#Barre sur le coté
table.plot(kind='bar')

plt.xlabel('Classe')
plt.ylabel('Proportion')
plt.title('Taux de survie selon la classe')
plt.legend(['Décédé', 'Survécu'])
plt.xticks(rotation=0)

plt.show()


#Barre emplilées

table.plot(kind='bar', stacked=True)

plt.xlabel('Classe')
plt.ylabel('Proportion')
plt.title('Répartition survie / décès selon la classe')
plt.legend(['Décédé', 'Survécu'])
plt.xticks(rotation=0)

plt.show()

```


.
