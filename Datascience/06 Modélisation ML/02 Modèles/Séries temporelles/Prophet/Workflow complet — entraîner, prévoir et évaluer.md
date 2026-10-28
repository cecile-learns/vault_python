# Prophet — workflow complet

## Pour faire quoi ?

Préparer une série, réserver une période de test, entraîner Prophet puis mesurer le RMSE.

## Script réutilisable

```python
import numpy as np
import pandas as pd
from prophet import Prophet
from sklearn.metrics import mean_squared_error

df_prophet = df[[col_date, col_valeur]].rename(
    columns={col_date: "ds", col_valeur: "y"}
)
df_prophet["ds"] = pd.to_datetime(df_prophet["ds"])

df_train = df_prophet.iloc[:-24].copy()
df_test = df_prophet.iloc[-24:].copy()

modele = Prophet(seasonality_mode="multiplicative")
modele.fit(df_train)

futur_test = df_test[["ds"]]
previsions_test = modele.predict(futur_test)

rmse = np.sqrt(mean_squared_error(df_test["y"], previsions_test["yhat"]))
print("RMSE :", rmse)
```

## À adapter

- colonnes source ;
- longueur du test ;
- fréquence et saisonnalité ;
- hyperparamètres retenus après validation temporelle.

## Vigilances & tips

Ne pas mélanger aléatoirement une série temporelle. Le test doit être postérieur au train.
