# Importer et indexer par le temps

## Pattern à l'import

```python
serie = pd.read_csv(
    "donnees.csv",
    parse_dates=["date"],
    index_col="date",
)
serie = serie.sort_index()

#cours
df = pd.read_csv('AirPassengers.csv', index_col='Month', parse_dates=True)
```

## Pattern après l'import

```python
df["date"] = pd.to_datetime(df["date"], errors="coerce")
df = df.dropna(subset=["date"]).sort_values("date")
df = df.set_index("date")
```

## Paramètres utiles

- `parse_dates` : colonnes à convertir en dates pendant la lecture ;
- `index_col` : colonne utilisée comme index ;
- `errors="coerce"` : transforme les dates illisibles en `NaT`, afin de les repérer ;
- `dayfirst=True` : interprète le jour avant le mois lorsque le format l'exige.

## Vérifications

```python
print(df.index.dtype)
print(df.index.is_monotonic_increasing)
print(df.index.duplicated().sum())
```

[Documentation officielle — `pandas.read_csv`](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html)
