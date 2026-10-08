# value_counts

## Syntaxe

```python
effectifs_par_categorie = df['col_categorie'].value_counts(dropna=False)


#Attention à ne pas confondre avec un besoin de len()
MHV_3=df[df['MedHouseVal']>3]
display(MHV_3)

print("Maison avec indicateurs > 3:",len(MHV_3))



# Fréquences
df['colonne'].value_counts(normalize=True)

```

## Paramètres

`dropna=False` : inclut les valeurs manquantes dans le comptage.

## Fonction

Compte les occurrences de chaque valeur.
affiche les différentes modalité d'une colonne et les compte
