# Prophet — entraîner et prévoir

## Pattern

```python
from prophet import Prophet

modele = Prophet(
    seasonality_mode="multiplicative",
    daily_seasonality=False,
    weekly_seasonality=False,
    yearly_seasonality=True,
)
modele.fit(df_train)

futur = modele.make_future_dataframe(periods=24, freq="MS")
previsions = modele.predict(futur)
```

## Paramètres utiles

- `periods` : nombre de périodes futures ;
- `freq` : fréquence Pandas, ici début de mois ;
- `seasonality_mode` : saisonnalité additive ou multiplicative ;
- `changepoint_prior_scale` : flexibilité de la tendance ;
- `seasonality_prior_scale` : flexibilité des saisonnalités ;
- `changepoint_range` : partie de l'historique autorisée à contenir des changements de tendance.

Les colonnes principales de sortie sont `yhat`, `yhat_lower` et `yhat_upper`.
