# Changer la fréquence — `resample`

## Quand l'utiliser ?

Pour agréger une série vers une fréquence plus large : données journalières vers données mensuelles, par exemple.

## Pattern

```python
serie_mensuelle = df.resample("ME")["ventes"].sum()
```

## Agrégations courantes

```python
df.resample("ME").sum()
df.resample("ME").mean()
df.resample("W").median()
```

## Paramètres et repères

- règle de fréquence : `"D"`, `"W"`, `"ME"`, `"QE"`, `"YE"` ;
- `on="date"` : utilise une colonne temporelle lorsque la date n'est pas l'index ;
- `closed` et `label` : contrôlent la fermeture et l'étiquette des intervalles.

Le choix de `sum`, `mean`, `last` ou d'une autre agrégation dépend du sens métier de la variable.

[Documentation officielle — `DataFrame.resample`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.resample.html)
