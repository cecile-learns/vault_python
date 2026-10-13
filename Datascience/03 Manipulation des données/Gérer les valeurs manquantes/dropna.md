# dropna

## Syntaxe

```python
df_nouveau = df.dropna(axis=0, how='any', subset=['col1', 'col2'])


df = df.dropna(subset=['left'])
df = df.loc[(df.isna().sum(axis=1)) < 3]
```

## Paramètres

`axis=0` cible les lignes ; `axis=1` les colonnes.

`how='any'` supprime dès qu’une valeur manque ; `how='all'` seulement si toutes manquent.

Avec `axis=0`, `subset` limite la recherche aux colonnes indiquées.

## Fonction

Supprime les lignes ou colonnes comportant des valeurs manquantes.

## Vigilances & tips

S’utilise souvent avec : `isna()`, `isnull()`, `notna()`.
