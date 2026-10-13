# `dt.to_timestamp()`

## Syntaxe

```python
serie_dates = df["col_periode"].dt.to_timestamp()
```

Choisir le début ou la fin de la période :

```python
serie_dates = df["col_periode"].dt.to_timestamp(how="start")
serie_dates = df["col_periode"].dt.to_timestamp(how="end")
```

## Paramètres

- `df["col_periode"]` : `Series` contenant des périodes pandas (`Period`).
- `.dt` : accesseur pandas permettant d'utiliser les méthodes temporelles sur une `Series`.
- `how="start"` : convertit chaque période en date correspondant au **début** de la période.
- `how="end"` : convertit chaque période en date correspondant à la **fin** de la période.

Sans `how`, pandas utilise le début de la période.

## Fonction

`dt.to_timestamp()` transforme une `Series` de périodes (`Period`) en dates (`Timestamp` / `datetime64`).

## Vigilances & tips

Une période ne représente pas un instant précis. Lors de la conversion, il faut donc choisir quel instant de la période représenter ; par défaut, pandas prend son début.

C'est l'opération inverse courante de `dt.to_period()`.

**S'utilise souvent avec :** `dt.to_period()`, `pd.to_datetime()`
