# columns

## Syntaxe

```python
noms_colonnes = df.columns

dictionnaire = {'A': [1, 5, 9], 
                'B': [2, 6, 10],
                'C': [3, 7, 11],
                'D': [4, 8, 12]}

df = pd.DataFrame(data = dictionnaire, index = ['i_1', 'i_2', 'i_3'])

print(df.columns)
```

>>> ['A', 'B', 'C', 'D']

## Fonction

Retourne l’index des noms de colonnes.

Retroune le nom de la colonne par rapport à son attribut colonne
