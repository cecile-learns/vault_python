# duplicated

## Syntaxe

```python
masque = df.duplicated(subset=['col_id'], keep=False)
```

## Paramètres

`keep=False` marque toutes les occurrences ; `subset` limite les colonnes comparées.

## Fonction

Repère les lignes dupliquées.

## Vigilances & tips

S’utilise souvent avec : `drop_duplicates()`.
