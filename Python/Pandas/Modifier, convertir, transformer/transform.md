# transform

## Syntaxe

```python
df['moyenne_groupe'] = df.groupby('col_groupe')['col_valeur'].transform('mean')
```

## Paramètres

`'col_groupe'` : colonne servant au regroupement.

`'col_valeur'` : colonne calculée.

`'mean'` : calcul appliqué à chaque groupe.

## Fonction

Calcule par groupe tout en retournant une valeur alignée sur chaque ligne d’origine.

## Vigilances & tips

S’utilise souvent avec : `groupby()`.
