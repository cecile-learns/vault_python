# Autres régresseurs — SVM, KNN et Random Forest

## Pattern commun

```python
modele.fit(X_train_modele, y_train)
y_pred = modele.predict(X_test_modele)
```

## Modèles rencontrés

```python
from sklearn.ensemble import RandomForestRegressor
from sklearn.neighbors import KNeighborsRegressor
from sklearn.svm import SVR

modeles = {
    "SVR": SVR(),
    "KNN": KNeighborsRegressor(),
    "Forêt": RandomForestRegressor(random_state=42),
}
```

## Vigilances & tips

SVR et KNN sont sensibles à l'échelle. La forêt ne nécessite généralement pas de standardisation. Comparer les modèles sur les mêmes partitions et métriques.
