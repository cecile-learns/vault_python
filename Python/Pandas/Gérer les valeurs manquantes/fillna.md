# fillna

## Syntaxe

```python
df_valeurs_completees = df.fillna({'col_age': df['col_age'].median(), 'col_ville': 'Inconnue'})


#Ajouter une colonne bonus
df['Bonus']=[None, 1000, None, 1500, None, 2000, None, 2500, None, 3000]

#Valeurs manquantes
df['Bonus'] = df['Bonus'].fillna(df['Bonus'].mean())

```


## Paramètres

Le dictionnaire associe chaque colonne à sa valeur de remplacement : médiane pour `col_age`, texte fixe pour `col_ville`.

## Fonction

Remplace les valeurs manquantes avec une stratégie adaptée à chaque colonne.

## Vigilances & tips

S’utilise souvent avec : `isna()`, `isnull()`, `notna()`.
