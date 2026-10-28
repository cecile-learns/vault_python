# drop_duplicates

## Syntaxe

```python
df_nouveau = df.drop_duplicates(subset=['col_id'], keep='first')
```

## Paramètres

`subset` limite les colonnes comparées ; `keep='first'`, `'last'` ou `False` choisit les occurrences conservées.

## Fonction

Supprime les lignes dupliquées.

## Vigilances & tips

S’utilise souvent avec : `duplicated()`.
