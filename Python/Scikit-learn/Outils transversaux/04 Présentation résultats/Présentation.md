
Afficher côte à côte les points véritablement observés pour chaque joueur, et les points prédits par le modèle
```python
pd.DataFrame({'points_observés': y_test, 'points_predits' : np.round(pred_test)}, 
             index=X_test.index).head(10)
```

