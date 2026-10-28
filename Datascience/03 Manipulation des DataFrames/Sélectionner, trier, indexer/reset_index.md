# reset_index

## Syntaxe

```python
df_index_retabli = df.reset_index(drop=False)
```

## Paramètres

`drop=False` : conserve l’ancien index comme colonne.

`drop=True` : abandonne l’ancien index.

## Fonction

Replace l’index dans les colonnes et crée un index numérique standard.

## Vigilances & tips

S’utilise souvent avec : `set_index()`.

Ici dans l'utilisation de group by pour récupérer le résultat de groupby sous une structure tabulaire avec la variable de regroupement comme colonne
```python
resume_groupes = df.groupby('col_groupe', as_index=False)['col_valeur'].mean()
#Series ; col_groupe devient l'index


df.groupby("col_groupe")["col_valeur"].mean().reset_index()
# → DataFrame ; col_groupe redevient une colonne
```