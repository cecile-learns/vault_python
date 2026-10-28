

![[Pasted image 20261010124833.png]]



Exemple de cours intéressants : 


```python
from sklearn.model_selection import KFold

# Définir les modèles à tester
models_cv = {
'clf_lr' : LogisticRegression(random_state=22, solver='liblinear', max_iter=3000),
'clf_rf' : RandomForestClassifier(random_state=22),
'clf_svc' : SVC(random_state=22)
}

# Définir le nombre de sous-enesemble (folds) pour KFold
n_splits = 5  # Par exemple, pour une validation croisée 5-fold
kf = KFold(n_splits=n_splits, shuffle=True, random_state=42)

# Stocker les résultats pour chaque modèle
results = {}

# Boucle sur chaque modèle
for model_name, model in models_cv.items():
    fold_accuracies = []  # Stocke les résultats de précision pour chaque pli (fold)
    
    # Effectuer la validation croisée KFold
    for train_index, test_index in kf.split(X):
        X_train, X_test = X.iloc[train_index], X.iloc[test_index]
        y_train, y_test = y.iloc[train_index], y.iloc[test_index]
        
        # Entraîner le modèle
        model.fit(X_train, y_train)
        
        # Prédire sur l'ensemble de test
        y_pred = model.predict(X_test)
        
        # Calculer la précision
        accuracy = accuracy_score(y_test, y_pred)
        fold_accuracies.append(accuracy)

    # Moyenne des résultats de précision pour tous les plis
    avg_accuracy = np.mean(fold_accuracies)
    var_accuracy = np.var(fold_accuracies, ddof=1)  # ddof=1 pour l'estimation non biaisée (échantillon)

    results[model_name] = {
        "mean_accuracy": avg_accuracy,
        "variance_accuracy": var_accuracy
    }
    
    
# Afficher les résultats
for model_name, metrics in results.items():
    print(f"Modèle : {model_name}")
    print(f"  - Average Accuracy : {metrics['mean_accuracy']:.2f}")
    print(f"  - Accuracy variance : {metrics['variance_accuracy']:.2f}")
```

![[Pasted image 20261010125253.png]]