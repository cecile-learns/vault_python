# `pd.Grouper()`

## Syntaxe

Regrouper une colonne temporelle par période :

```python
resultat = (
    df.groupby(pd.Grouper(key="col_date", freq="ME"))["col_valeur"]
      .mean()
)
```

Avec une autre variable de regroupement :

```python
resultat = (
    df.groupby([
        "col_groupe",
        pd.Grouper(key="col_date", freq="ME")
    ])["col_valeur"]
    .mean()
)
```

## Paramètres

- `key="col_date"` : colonne utilisée pour le regroupement temporel. Elle doit contenir des dates/heures exploitables par pandas.
- `freq="ME"` : fréquence des groupes temporels.
  - `"D"` : jour ;
  - `"W"` : semaine ;
  - `"ME"` : fin de mois ;
  - `"YE"` : fin d'année.
- `"col_groupe"` : dans la deuxième syntaxe, ajoute un premier niveau de regroupement avant la période temporelle.
- `["col_valeur"]` : colonne sur laquelle l'agrégation est calculée.
- `.mean()` : agrégation appliquée à chaque groupe.

## Fonction

Permet de définir des groupes, notamment **temporels**, à l'intérieur d'un `groupby()`.

## Vigilances & tips

`pd.Grouper()` définit **comment regrouper** ; ce n'est pas lui qui réalise l'agrégation. Il est particulièrement utile lorsqu'on combine une variable catégorielle et une fréquence temporelle.

**S'utilise souvent avec :** `groupby()`, `mean()`, `sum()`
