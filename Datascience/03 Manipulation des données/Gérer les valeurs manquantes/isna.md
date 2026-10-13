# isna

## Syntaxe

```python
masque = df.isna()
print(df.isna().sum())

(df.isna().sum() / len(df) * 100).round()
```

## Fonction

Repère les valeurs manquantes.

## Vigilances & tips

S’utilise souvent avec : `dropna()`, `fillna()`.


