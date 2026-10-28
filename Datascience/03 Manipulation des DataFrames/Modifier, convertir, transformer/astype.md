# astype

## Syntaxe

```python
df_nouveau = df.astype({'age': 'int64'})
df['NumOfProducts'].astype('object')
```

## Fonction

Convertit le dtype d’une ou plusieurs colonnes.


Attention peut aussi servir dans la concaténation notamment de date : 
carac['date']= carac['an'].astype(str) + '-' + carac['mois'].astype(str) + '-' + carac['jour'].astype(str)