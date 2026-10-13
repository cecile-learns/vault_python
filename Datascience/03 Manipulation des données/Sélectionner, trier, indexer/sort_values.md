# sort_values

## Syntaxe

```python
df_trie = df.sort_values(by=['col_ville', 'col_age'], ascending=[True, False])

tri_performance=df.sort_values(by=['Performance_Score', ascending=False])
display(tri_performance.head(3))
```

## Paramètres

`by` : colonnes utilisées successivement pour le tri.

`ascending=[True, False]` : ordre croissant pour `col_ville`, puis décroissant pour `col_age`.

## Fonction

Trie les lignes selon une ou plusieurs colonnes.


