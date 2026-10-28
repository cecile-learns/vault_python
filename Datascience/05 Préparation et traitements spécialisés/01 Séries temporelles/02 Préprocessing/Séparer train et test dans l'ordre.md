# Séparer train et test dans l'ordre

## Pattern simple

```python
n_test = 12

train = serie.iloc[:-n_test]
test = serie.iloc[-n_test:]
```


## Points de vigilance

- ne pas mélanger les observations ;
- placer le test après le train ;
- choisir un horizon comparable à l'usage réel du modèle ;
- refaire les transformations dans chaque fenêtre de validation ;
- garder un test final qui n'a servi à aucun choix de modèle.

[Documentation officielle — `TimeSeriesSplit`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html)
