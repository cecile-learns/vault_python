
# TPOT — Optimisation automatique de pipelines de Machine Learning

## Syntaxe

TPOT (_Tree-based Pipeline Optimization Tool_) est une bibliothèque d'**AutoML** qui recherche automatiquement des pipelines de Machine Learning à l'aide d'algorithmes évolutionnaires.

### Exemple — API TPOT 0.x (ancienne version)

```python
from tpot import TPOTClassifier
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.2,
    stratify=y,
    random_state=42
)

tpot = TPOTClassifier(
    generations=5,
    population_size=20,
    scoring="f1",
    cv=5,
    random_state=42,
    verbosity=2,
    n_jobs=-1
)

tpot.fit(X_train, y_train)

# Évaluation sur le test
score = tpot.score(X_test, y_test)

# Export du meilleur pipeline
tpot.export("meilleur_pipeline.py")
```

**Attention aux versions :** cet exemple correspond à l'ancienne API TPOT 0.x. TPOT 1.x a modifié son API et ses paramètres. Il faut vérifier la version installée avant de réutiliser ce code.

## Paramètres principaux — API 0.x

- `generations=5` : nombre de générations de pipelines évaluées.
    
- `population_size=20` : nombre de pipelines dans une génération.
    
- `offspring_size` : nombre de nouveaux pipelines produits à chaque génération.
    
- `scoring="f1"` : métrique utilisée pour comparer les pipelines.
    
- `cv=5` : nombre de folds de validation croisée.
    
- `random_state=42` : reproductibilité des opérations aléatoires.
    
- `verbosity=2` : niveau d'affichage de la progression.
    
- `n_jobs=-1` : utilisation des cœurs CPU disponibles.
    
- `max_time_mins` : limite de durée totale de recherche.
    
- `max_eval_time_mins` : limite de durée pour l'évaluation d'un pipeline.
    

## Fonction

TPOT automatise la recherche d'un pipeline performant en explorant différentes combinaisons de traitements et de modèles.

Il peut notamment rechercher :

- Des méthodes de prétraitement.
    
- Des méthodes de sélection de variables.
    
- Des algorithmes de classification ou de régression.
    
- Des hyperparamètres adaptés aux algorithmes sélectionnés.
    

### Principe

```
Données d'entraînement
          ↓
Génération de pipelines candidats
          ↓
Évaluation par validation croisée
          ↓
Sélection des meilleurs pipelines
          ↓
Mutations et croisements
          ↓
Nouvelles générations
          ↓
Meilleur pipeline identifié
```

La recherche évolutionnaire s'inspire des mécanismes de sélection, mutation et croisement.

## Classification et régression

|Problème|Classe TPOT|
|---|---|
|Classification|`TPOTClassifier`|
|Régression|`TPOTRegressor`|

Les métriques doivent être adaptées au problème :

- Classification : `accuracy`, `f1`, `balanced_accuracy`, `roc_auc`.
    
- Régression : `r2`, `neg_mean_absolute_error`, `neg_root_mean_squared_error`.
    

## Résultats

Dans l'ancienne API 0.x :

```
# Prédictions
y_pred = tpot.predict(X_test)

# Score sur le test
score = tpot.score(X_test, y_test)

# Pipeline sélectionné
pipeline = tpot.fitted_pipeline_

# Export du code Python
tpot.export("meilleur_pipeline.py")
```

Le fichier exporté permet d'inspecter et de réutiliser le pipeline retenu.

## Comparaison avec les autres méthodes

|Méthode|Ce qu'elle recherche|
|---|---|
|`GridSearchCV`|La meilleure combinaison dans une grille d'hyperparamètres|
|`RandomizedSearchCV`|Une bonne combinaison parmi des tirages aléatoires|
|`BayesSearchCV`|Des hyperparamètres par optimisation bayésienne|
|TPOT|Des combinaisons de modèles, transformations et hyperparamètres|

**Différence essentielle :** TPOT peut rechercher la structure du pipeline, et pas uniquement les hyperparamètres d'un modèle déjà choisi.

## Vigilances & tips

- **Coût de calcul** : l'évaluation de nombreux pipelines sur plusieurs folds peut être longue.
    
- **Versions** : ne pas mélanger les exemples TPOT 0.x et TPOT 1.x.
    
- **Prétraitement** : TPOT ne garantit pas que toutes les opérations métier nécessaires soient automatiquement réalisées.
    
- **Données manquantes et catégorielles** : vérifier les exigences des pipelines et de la version utilisée.
    
- **Fuite de données** : conserver le test à part et intégrer les transformations apprises dans la validation croisée.
    
- **Déséquilibre des classes** : choisir une métrique adaptée et vérifier explicitement la stratégie de rééchantillonnage.
    
- **Interprétabilité** : un pipeline performant peut être complexe à expliquer.
    
- **Surajustement de la recherche** : tester de nombreuses configurations peut conduire à sélectionner un pipeline trop adapté aux folds de validation.
    
- **Reproductibilité** : fixer les graines aléatoires et conserver les versions des bibliothèques.
    

**S'utilise avec :** `scikit-learn`, `Pipeline`, validation croisée, métriques de classification et de régression.