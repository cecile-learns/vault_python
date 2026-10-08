# merge

## Syntaxe

```python
df_nouveau = df1.merge(df2, on=['col_id]', how='left')
```

## Paramètres

`how='left'`, `'inner'`, `'right'` ou `'outer'` détermine les lignes conservées.

## Exemple

```python
df_nouveau = df1.merge(
    df2,
    left_on='id_client',
    right_on='client_id',
    how='left'
)
```

## Fonction

Fusionne deux DataFrames selon une ou plusieurs clés.


```python

test_gre=pd.cut(df['gre'],
               bins=[200,450,550,620,800],
               labels=['mauvais', 'moyen', 'moyen+', 'bon'])
df=df.merge(test_gre,left_index=True, right_index=True)
df = df.drop(columns='gre_x', axis = 1)
df = df.rename({'gre_y' : 'gre'}, axis=1)
```

