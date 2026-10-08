

# GridSearchCV

## Syntaxe

```python
from sklearn.model_selection import GridSearchCV

grille_parametres = {
    "hyperparametre_1": [valeur_1, valeur_2],
    "hyperparametre_2": [valeur_1, valeur_2]
}

recherche = GridSearchCV(
    estimator=modele_a_regler,
    param_grid=grille_parametres,
    cv=5,
    scoring="recall"
)

recherche.fit(X_train, y_train)
````

## Paramètres

- `estimator` : modèle dont on cherche les meilleurs hyperparamètres.
- `param_grid` : dictionnaire contenant les hyperparamètres à tester et leurs valeurs possibles.
- `cv` : nombre de plis de validation croisée.
- `scoring` : métrique utilisée pour choisir la meilleure combinaison.
- `refit=True` : par défaut, réentraîne le meilleur modèle sur tout le jeu d'entraînement.

### Choisir `scoring`

Le choix dépend de la finalité métier :
Classification
- `"accuracy"` : proportion globale de bonnes prédictions.
- `"recall"` : détecter un maximum de vrais positifs.
- `"precision"` : limiter les faux positifs.
- `"f1"` : équilibre entre précision et rappel.
- `"roc_auc"` : capacité à distinguer les deux classes.

Regression 
- `r2` : coefficient de détermination. Plus c’est proche de 1, mieux c’est. Peut être négatif si le modèle fait pire qu’une prédiction naïve.
- `neg_mean_absolute_error` : MAE. Erreur absolue moyenne. Plus l’erreur réelle est petite, mieux c’est.
- `neg_mean_squared_error` : MSE. Pénalise davantage les grosses erreurs.
- `neg_root_mean_squared_error` : RMSE. Même logique que MSE, mais dans l’unité de la cible, donc souvent plus facile à interpréter.
- `neg_mean_absolute_percentage_error` : MAPE. Erreur relative en pourcentage, utile seulement si la cible n’est pas proche de 0.

## Résultats utiles

```python
recherche.best_params_
recherche.best_score_
recherche.best_estimator_
recherche.cv_results_
```

- `best_params_` : meilleure combinaison testée.
- `best_score_` : meilleur score moyen obtenu en validation croisée.
- `best_estimator_` : modèle avec les meilleurs hyperparamètres.
- `cv_results_` : résultats détaillés de toutes les combinaisons testées.

## Prédire sur le test

```python
y_pred = recherche.predict(X_test)
```

Le jeu de test sert à évaluer le modèle final, pas à choisir les hyperparamètres.

## Fonction

Teste différentes combinaisons d'hyperparamètres par validation croisée afin de sélectionner le meilleur réglage selon la métrique choisie.

## Vigilances & tips

- Les clés de `param_grid` doivent correspondre exactement aux hyperparamètres du modèle.
- GridSearchCV ne teste que les valeurs présentes dans la grille.
- Le « meilleur » modèle dépend directement du `scoring` choisi.
- Plus la grille contient de combinaisons et plus `cv` est élevé, plus le nombre d'entraînements augmente.
- Si un prétraitement doit lui-même être appris (`StandardScaler`, imputation…), son intégration correcte dans la validation croisée sera gérée avec les `Pipeline`.

**S'utilise souvent avec :** `fit()`, `predict()`, `classification_report()`



```Python
#Exemple du cours
#Création d'un dictionnaires avec les valeurs possibles pour les paramètres
parametres = {'C':[0.1,1,10], 'kernel':['rbf','linear', 'poly'], 'gamma':[0.001, 0.1, 0.5]}

#Appliquer la fonction au modèle clf en lui spécifiant dans l'argument param-grid, la grille de paramètre que l'on a créée. L'argument scoring permet de choisir la métrique que l'on souhaite utliiser pour évaluer la performance des modèles
grid_clf = model_selection.GridSearchCV(estimator=clf, param_grid=parametres)

#Entrainer grid-clf sur l'ensemble d'entrainement donc ici **`X_train_scaled, y_train`**
grille = grid_clf.fit(X_train_scaled,y_train)

#Afficher toutes les combinaison possible et la performance moyenne
print(pd.DataFrame.from_dict(grille.cv_results_).loc[:,['params', 'mean_test_score']]) 

#Afficher les meilleurs paramètre de la grille
print(grid_clf.best_params_)

#Effectuer les prédictions de classes et afficher la matrice de confusion
y_pred = grid_clf.predict(X_test_scaled)
pd.crosstab(y_test, y_pred, rownames=['Classe réelle'], colnames=['Classe prédite'])


```
