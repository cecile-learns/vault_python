# crosstab

## Syntaxe

```python
tableau_effectifs = pd.crosstab(df['col_categorie'], df['col_classe'])
```

## Paramètres

`col_categorie` détermine les lignes ; `col_classe` détermine les colonnes.

`normalize='index'` calcule des proportions par ligne ; 
normalize='columns' calcule des proportion par colonne
`margins=True` ajoute les totaux.

## Exemple

```python
tableau_proportions = pd.crosstab(
    df['col_categorie'],
    df['col_classe'],
    normalize='index'
)
```

## Fonction

Crée un tableau de contingence entre deux variables.
