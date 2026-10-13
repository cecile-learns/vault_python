# Opérateurs binaires

## Syntaxe

```python
condition_age = df['col_age'] >= 18
condition_ville = df['col_ville'] == 'Bordeaux'

lignes_selectionnees = df[condition_age & condition_ville]
lignes_alternatives = df[condition_age | condition_ville]
lignes_inversees = df[~condition_ville]
```

## Paramètres

- `==`, `!=`, `>`, `>=`, `<`, `<=` : comparent chaque valeur d’une Series.
- `&` : ET élément par élément.
- `|` : OU élément par élément.
- `~` : inverse une condition booléenne.

## Fonction

Construit et combine des masques booléens pour filtrer les lignes d’un DataFrame.

## Vigilances & tips

Chaque condition composée doit être placée entre parenthèses lorsqu’elle est écrite directement dans le filtre.

```python
display(df
[
(df['Department'] == 'IT') & 
(df['Performance_Score'] > 85)]
)

```