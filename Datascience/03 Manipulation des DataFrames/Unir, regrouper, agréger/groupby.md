# groupby

## Syntaxe

```python
resume_groupes = df.groupby('col_groupe', as_index=False)['col_valeur'].mean()
#Series ; col_groupe devient l'index


df.groupby("col_groupe")["col_valeur"].mean().reset_index()
# → DataFrame ; col_groupe redevient une colonne
```
```


```

## Paramètres

`'col_groupe'` : colonne dont les valeurs servent à constituer les groupes.

`as_index=False` : conserve la colonne de regroupement comme colonne du résultat.

`['col_valeur']` : colonne sur laquelle le calcul est effectué.

`.mean()` : calcule ici la moyenne de chaque groupe.

## Fonction

Regroupe les lignes qui partagent la même valeur, puis calcule ici une moyenne par groupe.

## Vigilances & tips

S’utilise souvent avec : `agg()`, `mean()`, `sum()`, `count()`.
