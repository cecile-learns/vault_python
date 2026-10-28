# isin

## Syntaxe

```python
lignes_villes_cibles = df[df['col_ville'].isin(['Paris', 'Lyon'])]
```

## Paramètres

`['Paris', 'Lyon']` : valeurs acceptées dans `col_ville`.

## Fonction

Conserve les lignes dont la valeur appartient à une collection.
