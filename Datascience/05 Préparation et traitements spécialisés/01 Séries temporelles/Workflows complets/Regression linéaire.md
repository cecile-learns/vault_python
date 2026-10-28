
## Importer les données et librairies
```python
import pandas as pd
import matplotlib.pyplot as plt

from sklearn.linear_model import LinearRegression
from sklearn.metrics import root_mean_squared_error, r2_score

df = pd.read_csv('AirPassengers.csv', index_col='Month', parse_dates=True)

```

## Préparation des données
```python
df['Passengers_lag1']=df['#Passengers'].shift(1)
df.dropna()
```

## Mise en place du modèle de régression linéaire

```python
y=df['#Passengers']
X=df['Passengers_lag1']

#Séparation sur les 24 derniers mois
X_train=X.iloc[:-24]
X_test=X.iloc[-24:]

y_train=y.iloc[:-24]
y_test=y.iloc[-24:]

model = LinearRegression()
model.fit(X_train, y_train)

y_pred = model.predict(X_test)

rmse = root_mean_squared_error(y_test, y_pred)
print(f'Erreur quadratique moyenne (RMSE) : {rmse}')

r2 = r2_score(y_test, y_pred)
print(f'Coefficient de détermination (R²) : {r2}')

plt.figure(figsize=(10, 6))
plt.plot(y.index, y, label='Valeurs réelles')
plt.plot(y_test.index, y_pred, label='Prédictions', linestyle='--')
plt.title('Prédictions de la régression linéaire')
plt.xlabel('Date')
plt.ylabel('Nombre de passagers')
plt.legend()
plt.show()


```
![[Pasted image 20261017112337 1.png]]

## Interprétabilité

```python
coefficients = model.coef_
intercept = model.intercept_

print('Modèle construit :')
print(f"Xₜ = {coefficients[0]:.2f} × Xₜ₋₁ + {intercept:.2f}")
```
Modèle construit :
Xₜ = 0.95 × Xₜ₋₁ + 14.26

