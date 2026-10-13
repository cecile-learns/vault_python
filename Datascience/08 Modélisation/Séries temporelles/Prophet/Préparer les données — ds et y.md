# Préparer les données Prophet — `ds` et `y`

## Pattern

```python
df_prophet = df[[col_date, col_valeur]].rename(
    columns={col_date: "ds", col_valeur: "y"}
)
df_prophet["ds"] = pd.to_datetime(df_prophet["ds"])
df_prophet["y"] = pd.to_numeric(df_prophet["y"])
```

Prophet attend `ds` pour les dates et `y` pour la valeur numérique à prévoir.

Documentation : [Quick Start Prophet](https://facebook.github.io/prophet/docs/quick_start.html)
