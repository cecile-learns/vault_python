# Bayesian Search — `BayesSearchCV`

## Syntaxe

```python
from skopt import BayesSearchCV
from skopt.space import Integer, Real, Categorical
from sklearn.ensemble import RandomForestClassifier

modele = RandomForestClassifier(random_state=42)

parametres = {
    "n_estimators": Integer(50, 300),
    "max_depth": Integer(3, 30),
    "min_samples_split": Integer(2, 20),
    "max_features": Categorical(["sqrt", "log2"])
}

recherche = BayesSearchCV(
    estimator=modele,
    search_spaces=parametres,
    n_iter=30,
    scoring="f1",
    cv=5,
    random_state=42,
    n_jobs=-1,
    refit=True
)

recherche.fit(X_train, y_train)

meilleurs_parametres = recherche.best_params_
meilleur_score = recherche.best_score_
meilleur_modele = recherche.best_estimator_

y_pred = meilleur_modele.predict(X_test)
```

## Paramètres

- `estimator` : modèle dont on souhaite optimiser les hyperparamètres.
    
- `search_spaces` : espace des hyperparamètres à explorer.
    
- `n_iter=30` : nombre d'itérations de recherche.
    
- `scoring="f1"` : métrique utilisée pour comparer les configurations.
    
- `cv=5` : validation croisée à 5 folds.
    
- `random_state=42` : rend la recherche reproductible.
    
- `n_jobs=-1` : utilise tous les processeurs disponibles pour paralléliser les évaluations.
    
- `refit=True` : réentraîne le meilleur modèle sur l'ensemble du train.
    
- `n_points=1` : nombre de configurations évaluées en parallèle à chaque étape de recherche.
    

## Choisir les paramètres

### `search_spaces`

Trois types de distributions :

```
from skopt.space import Integer, Real, Categorical

parametres = {
    "max_depth": Integer(3, 30),
    "learning_rate": Real(0.001, 0.3, prior="log-uniform"),
    "max_features": Categorical(["sqrt", "log2"])
}
```

|Distribution|Type de valeurs|Exemple|
|---|---|---|
|`Integer(a, b)`|Entiers entre `a` et `b`, bornes incluses|Profondeur d'un arbre|
|`Real(a, b)`|Valeurs réelles continues|Taux d'apprentissage|
|`Categorical([...])`|Valeurs prédéfinies|Type de critère ou stratégie|

`prior="log-uniform"` est utile pour les paramètres positifs dont les valeurs pertinentes peuvent varier sur plusieurs ordres de grandeur, comme un taux d'apprentissage.

### `n_iter`

|Valeur|Usage|
|---|---|
|10|Exploration rapide|
|30–50|Recherche intermédiaire|
|100 ou plus|Recherche approfondie|

Ce sont des repères pratiques, pas des seuils universels.

Le coût approximatif est :

**Nombre d'entraînements CV =** `**n_iter × cv**`

Exemple : `30 × 5 = 150` entraînements, auxquels s'ajoute le réentraînement final si `refit=True`.

### `scoring` selon la cible

|Problème|Métriques possibles|
|---|---|
|Classification équilibrée|`accuracy`|
|Classification déséquilibrée|`f1`, `balanced_accuracy`|
|Éviter les faux négatifs|`recall`|
|Éviter les faux positifs|`precision`|
|Qualité du classement binaire|`roc_auc`, `average_precision`|
|Régression|`r2`, `neg_mean_absolute_error`, `neg_root_mean_squared_error`|

Le choix dépend de l'objectif métier.

## Résultats

```
recherche.best_params_      # Meilleure combinaison
recherche.best_score_       # Score moyen en validation croisée
recherche.best_estimator_   # Modèle réentraîné sur le train
recherche.cv_results_       # Résultats détaillés
```

Pour consulter les meilleures configurations :

```
import pandas as pd

resultats = pd.DataFrame(recherche.cv_results_)

classement = resultats.sort_values(
    "rank_test_score"
)[[
    "params",
    "mean_test_score",
    "std_test_score",
    "rank_test_score"
]].head(10)
```

## Fonction

`BayesSearchCV` réalise une **optimisation bayésienne des hyperparamètres**.

Contrairement à Random Search, qui tire les combinaisons aléatoirement, Bayesian Search utilise les résultats des évaluations précédentes pour orienter les prochaines configurations à tester.

Son fonctionnement :

1. Tester quelques configurations initiales.
    
2. Construire un modèle probabiliste estimant les performances selon les hyperparamètres.
    
3. Choisir une nouvelle configuration en équilibrant exploration et exploitation.
    
4. Évaluer cette configuration par validation croisée.
    
5. Actualiser le modèle probabiliste et recommencer.
    

L'objectif est de trouver de bonnes configurations avec un nombre limité d'évaluations.

## Comparaison des méthodes

|Méthode|Stratégie|Particularité|
|---|---|---|
|`GridSearchCV`|Toutes les combinaisons définies|Exhaustif sur la grille|
|`RandomizedSearchCV`|Tirages aléatoires|Budget contrôlé, explorations indépendantes|
|`BayesSearchCV`|Recherche adaptative|Utilise les résultats précédents pour orienter les essais|

## Vigilances & tips

- **Bibliothèque externe** : `BayesSearchCV` appartient à `scikit-optimize` (`skopt`), pas directement à scikit-learn. Installation : `pip install scikit-optimize`.
    
- **Compatibilité** : vérifier la compatibilité de `scikit-optimize` avec les versions installées de Python, NumPy et scikit-learn.
    
- **Pas de garantie d'optimum global** : la recherche dépend de l'espace défini, du budget et des évaluations précédentes.
    
- **Espace de recherche** : des bornes mal choisies peuvent empêcher de trouver de bons hyperparamètres.
    
- **Temps de calcul** : l'optimisation bayésienne ajoute un coût de calcul pour choisir les configurations suivantes ; elle n'est pas systématiquement plus rapide.
    
- **Test final isolé** : ajuster `BayesSearchCV` uniquement sur le train. Le test sert à évaluer la généralisation après optimisation.
    
- **Preprocessing** : intégrer les transformations apprises dans un `Pipeline` pour éviter les fuites entre folds.
    
- **Validation croisée** : adapter `cv` au problème (`StratifiedKFold`, `TimeSeriesSplit`, etc.).
    
- **Pipeline** : utiliser la syntaxe `nom_etape__parametre`, par exemple `"modele__max_depth"`.
    
- **Parallélisation** : `n_jobs` parallélise les évaluations ; `n_points` contrôle le nombre de configurations proposées simultanément. Une recherche fortement parallélisée est moins séquentielle.
    

**S'utilise souvent avec :** `Pipeline`, `RandomizedSearchCV`, `GridSearchCV`, `cross_validate()`