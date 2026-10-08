
# Random Search — `RandomizedSearchCV`

## Syntaxe

```python
from sklearn.model_selection import RandomizedSearchCV
from sklearn.ensemble import RandomForestClassifier
from scipy.stats import randint

modele = RandomForestClassifier(random_state=42)

parametres = {
    "n_estimators": randint(50, 301),
    "max_depth": [None, 5, 10, 20],
    "min_samples_split": randint(2, 21)
}

recherche = RandomizedSearchCV(
    estimator=modele,
    param_distributions=parametres,
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
    
- `param_distributions` : dictionnaire des hyperparamètres à explorer et des valeurs ou distributions possibles.
    
- `n_iter=30` : nombre de combinaisons d'hyperparamètres testées.
    
- `scoring="f1"` : métrique utilisée pour comparer les combinaisons.
    
- `cv=5` : validation croisée à 5 folds.
    
- `random_state=42` : rend les tirages reproductibles.
    
- `n_jobs=-1` : utilise tous les processeurs disponibles pour paralléliser les calculs.
    
- `refit=True` : réentraîne automatiquement le meilleur modèle sur l'ensemble du train.
    

## Choisir les paramètres

### `param_distributions`

Deux possibilités :

```
# Valeurs prédéfinies
"max_depth": [None, 5, 10, 20]

# Distribution aléatoire d'entiers
"n_estimators": randint(50, 301)
```

- **Liste** : adaptée à quelques valeurs précises à comparer.
    
- **Distribution** : adaptée à une plage de valeurs à explorer.
    

`randint(50, 301)` tire des entiers entre 50 et 300 inclus.

### `n_iter`

|Valeur|Usage|
|---|---|
|10|Exploration rapide|
|30–50|Recherche intermédiaire|
|100 ou plus|Recherche approfondie, plus coûteuse|

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

Le choix dépend de l'objectif métier, pas uniquement du type de modèle.

## Résultats

```
recherche.best_params_      # Meilleure combinaison
recherche.best_score_       # Score moyen en validation croisée
recherche.best_estimator_   # Modèle réentraîné sur le train
recherche.cv_results_       # Résultats détaillés
```

Pour consulter les meilleures combinaisons :

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

`RandomizedSearchCV` recherche une bonne combinaison d'**hyperparamètres** en tirant aléatoirement un nombre défini de configurations.

Chaque configuration est évaluée par validation croisée.

Contrairement à `GridSearchCV`, qui explore toutes les combinaisons définies, Random Search n'en teste qu'un échantillon.

Il permet donc d'explorer des espaces d'hyperparamètres importants avec un budget de calcul contrôlé.

## Vigilances & tips

- **Pas de garantie d'optimum** : la meilleure combinaison trouvée est seulement la meilleure parmi celles testées.
    
- **Test final isolé** : `RandomizedSearchCV` doit être ajusté sur `X_train` et `y_train` uniquement. Le test sert à évaluer le modèle final, pas à choisir les hyperparamètres.
    
- **Preprocessing** : intégrer les transformations apprises dans un `Pipeline` pour éviter les fuites entre folds.
    
- **Validation croisée** : adapter `cv` au problème (`StratifiedKFold` pour une classification, `TimeSeriesSplit` pour des données temporelles, etc.).
    
- **Classes déséquilibrées** : `f1` ne convient pas systématiquement ; choisir la métrique selon le coût des erreurs et définir la classe positive pertinente.
    
- **Budget de calcul** : commencer avec peu d'itérations, puis élargir la recherche si les résultats le justifient.
    
- **Hyperparamètres d'un Pipeline** : utiliser la syntaxe `nom_etape__parametre`, par exemple `"modele__max_depth"`.
    

**S'utilise souvent avec :** `Pipeline`, `GridSearchCV`, `cross_validate()`, `StratifiedKFold`