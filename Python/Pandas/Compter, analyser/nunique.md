display# nunique

## Syntaxe

```python
nombre_categories = df['col_categorie'].nunique(dropna=True)
```

## Paramètres

`dropna=True` : exclut les valeurs manquantes du nombre obtenu.

## Fonction

Compte le nombre de valeurs distinctes.

## Vigilances & tips

S’utilise souvent avec : `unique()`, `value_counts()`.
