# drop

## Syntaxe

```python
df_sans_colonnes = df.drop(columns=['col_inutile'])
```
```
df = df.drop(df[df['AveRooms'] > 10].index)
```
## Paramètres

`columns=[...]` : noms des colonnes à supprimer.

`index=[...]` : labels des lignes à supprimer.
 Attention drop attend les index de ligne ou les labels des colonnes
## Fonction

Supprime des lignes ou des colonnes.
