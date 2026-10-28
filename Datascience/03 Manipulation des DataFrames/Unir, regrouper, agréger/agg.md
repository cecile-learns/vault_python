# agg

## Syntaxe

```python
resume_groupes = df.groupby('col_groupe').agg(moyenne=('col_valeur', 'mean'), total=('col_valeur', 'sum'))
```

## Paramètres

`moyenne=...` et `total=...` : noms des colonnes produites.

`'col_valeur'` : colonne analysée.

`'mean'` et `'sum'` : agrégations appliquées.

## Fonction

Calcule plusieurs agrégations et attribue des noms explicites aux résultats.

## Vigilances & tips

S’utilise souvent avec : `groupby()`.
