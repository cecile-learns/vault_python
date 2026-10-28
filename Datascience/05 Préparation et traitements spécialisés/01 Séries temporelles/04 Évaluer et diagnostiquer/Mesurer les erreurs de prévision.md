# Mesurer les erreurs de prévision

## Pattern

```python
from sklearn.metrics import mean_absolute_error, mean_absolute_percentage_error, mean_squared_error

mae = mean_absolute_error(y_test, y_pred)
rmse = mean_squared_error(y_test, y_pred) ** 0.5
mape = mean_absolute_percentage_error(y_test, y_pred)
```

## Choisir la métrique

- **MAE** : erreur moyenne dans l'unité de la cible ;
- **RMSE** : pénalise davantage les grandes erreurs ;
- **MAPE** : erreur relative, difficile à interpréter lorsque les valeurs réelles sont nulles ou proches de zéro.

Comparer le modèle à une prévision de référence, par exemple la dernière valeur ou la valeur de la saison précédente.

[Documentation officielle — métriques de régression](https://scikit-learn.org/stable/api/sklearn.metrics.html#regression-metrics)
