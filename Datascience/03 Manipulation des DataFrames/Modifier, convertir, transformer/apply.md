# apply

## Syntaxe

```python
texte_normalise = df['col_texte'].apply(fonction_normalisation)

df['col_categorie_age'] = df['col_age'].apply(
    lambda age: 'adulte' if age >= 18 else 'mineur'
)
```

## Paramètres

`fonction_normalisation` : fonction appelée pour chaque valeur.

`lambda age: ...` : petite fonction anonyme appliquée ici à chaque valeur de `col_age`.

Sur un DataFrame, `axis=0` travaille par colonne et `axis=1` par ligne.

## Fonction

Applique une fonction à une Series ou, selon un axe, à un DataFrame.

## Vigilances & tips

S’utilise souvent avec : `map()`, `transform()`.
