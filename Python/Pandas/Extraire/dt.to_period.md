# `dt.to_period()`

## Syntaxe

```python
serie_periodes = df["col_date"].dt.to_period("M")
```

Autres fréquences courantes :

```python
serie_periodes = df["col_date"].dt.to_period("D")  # jour
serie_periodes = df["col_date"].dt.to_period("Y")  # année
```

## Paramètres

- `df["col_date"]` : `Series` contenant des dates au format datetime.
- `.dt` : accesseur pandas permettant d'utiliser les propriétés et méthodes liées aux dates sur une `Series`.
- `"M"` : fréquence de la période créée ; ici une période mensuelle.
- `"D"` : période correspondant à un jour.
- `"Y"` : période correspondant à une année.

## Fonction

`dt.to_period()` transforme chaque date en **période temporelle** selon la fréquence choisie. Le résultat est une `Series` de périodes (`Period`), et non plus de dates précises.

## Vigilances & tips

Avec `"M"`, toutes les dates d'un même mois deviennent par exemple `2026-09` : on conserve la période, pas le jour précis.

Pour revenir à des dates (`Timestamp`), utiliser `dt.to_timestamp()`.

**S'utilise souvent avec :** `pd.to_datetime()`, `dt.to_timestamp()`, `groupby()`
