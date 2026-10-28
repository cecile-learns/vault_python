# concat

## Syntaxe

```python
df_nouveau = pd.concat([df1, df2], axis=0, ignore_index=True)
```

## Paramètres

`axis=0` empile les lignes ; `axis=1` juxtapose les colonnes.

`ignore_index=True` recrée un index séquentiel.

## Fonction

Assemble des objets Pandas le long d’un axe.
