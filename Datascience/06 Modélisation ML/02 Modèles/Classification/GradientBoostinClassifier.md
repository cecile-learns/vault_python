
# GradientBoostingClassifier — Classification par Gradient Boosting

## Syntaxe

```python
from sklearn.ensemble import GradientBoostingClassifier

gb = GradientBoostingClassifier(
    n_estimators=100,
    learning_rate=0.1,
    max_depth=3,
    subsample=1.0,
    random_state=42
)

# Entraînement
gb.fit(X_train, y_train)

# Prédictions
y_pred = gb.predict(X_test)

# Probabilités prédites
y_proba = gb.predict_proba(X_test)
```

## Paramètres principaux

- `n_estimators=100` : nombre d'étapes de boosting, généralement un arbre ajouté à chaque étape en classification binaire.
    
- `learning_rate=0.1` : contrôle la contribution de chaque nouvel arbre au modèle.
    
- `max_depth=3` : profondeur maximale des arbres de décision utilisés.
    
- `subsample=1.0` : proportion des observations utilisées à chaque étape. Une valeur inférieure à 1 introduit un échantillonnage aléatoire sans remise.
    
- `min_samples_split=2` : nombre minimal d'observations nécessaires pour diviser un nœud.
    
- `min_samples_leaf=1` : nombre minimal d'observations dans une feuille.
    
- `max_features=None` : nombre de variables considérées lors de la recherche d'une séparation.
    
- `loss="log_loss"` : fonction de perte optimisée pour la classification probabiliste.
    
- `validation_fraction=0.1` : proportion des données réservées à la validation interne lorsque l'arrêt anticipé est activé.
    
- `n_iter_no_change=None` : nombre d'itérations sans amélioration avant l'arrêt anticipé ; `None` le désactive.
    
- `random_state=42` : reproductibilité des opérations aléatoires.
    

## Fonction

`GradientBoostingClassifier` est un algorithme d'apprentissage supervisé utilisé pour la **classification binaire ou multiclasse**.

Il appartient à la famille des méthodes d'ensemble de type _Boosting_.

Contrairement au Random Forest, qui entraîne plusieurs arbres relativement indépendants, le Gradient Boosting construit les arbres **successivement**.

Chaque nouvel arbre cherche à corriger les erreurs du modèle précédent en suivant le gradient de la fonction de perte.

### Principe

```
Modèle initial
      ↓
Calcul des erreurs via la fonction de perte
      ↓
Nouvel arbre ajusté aux pseudo-résidus
      ↓
Ajout de sa contribution au modèle
      ↓
Mise à jour des prédictions
      ↓
Répétition jusqu'au nombre d'étapes prévu
```

Les pseudo-résidus correspondent au gradient négatif de la fonction de perte. Ce ne sont pas nécessairement de simples différences entre les classes réelles et prédites.

## Exemple d'utilisation

```python
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.metrics import classification_report, confusion_matrix

gb = GradientBoostingClassifier(
    n_estimators=200,
    learning_rate=0.05,
    max_depth=3,
    random_state=42
)

gb.fit(X_train, y_train)

y_pred = gb.predict(X_test)

print(confusion_matrix(y_test, y_pred))
print(classification_report(y_test, y_pred))
```


## Choisir les hyperparamètres

|Paramètre|Valeur faible|Valeur élevée|
|---|---|---|
|`n_estimators`|Moins d'étapes, risque de sous-apprentissage|Plus de capacité, coût et risque de surapprentissage|
|`learning_rate`|Apprentissage progressif, souvent plus d'arbres nécessaires|Apprentissage plus rapide, risque d'instabilité|
|`max_depth`|Relations simples|Interactions plus complexes|
|`subsample`|Plus d'aléatoire, régularisation possible|Utilisation de davantage d'observations|

**Interaction importante :** `learning_rate` et `n_estimators` doivent être ajustés conjointement. Diminuer `learning_rate` nécessite souvent d'augmenter le nombre d'étapes.

### Optimisation avec GridSearchCV

```python
from sklearn.model_selection import GridSearchCV

param_grid = {
    "n_estimators": [100, 200],
    "learning_rate": [0.05, 0.1],
    "max_depth": [2, 3, 5],
    "subsample": [0.8, 1.0]
}

grid = GridSearchCV(
    estimator=GradientBoostingClassifier(random_state=42),
    param_grid=param_grid,
    scoring="f1",
    cv=5,
    n_jobs=-1
)

grid.fit(X_train, y_train)

print(grid.best_params_)
print(grid.best_score_)
```

`scoring="f1"` convient notamment à une classification binaire lorsque la classe positive et l'équilibre précision/rappel sont pertinents. Le choix doit rester lié à l'objectif métier.

## Comparaison avec RandomForestClassifier

|Critère|Random Forest|Gradient Boosting|
|---|---|---|
|Famille|Bagging|Boosting|
|Construction|Arbres largement indépendants|Arbres successifs|
|Objectif|Réduire notamment la variance|Réduire progressivement la perte|
|Parallélisation des arbres|Facile|Plus limitée|
|Hyperparamètres sensibles|Profondeur, nombre d'arbres, feuilles|Learning rate, nombre d'étapes, profondeur|
|Surapprentissage|Généralement assez robuste|Sensible aux réglages|

## Vigilances & tips

- **Surapprentissage** : surveiller notamment `n_estimators`, `max_depth` et `learning_rate`.
    
- **Scaling** : généralement inutile pour les arbres de décision.
    
- **Variables catégorielles** : nécessitent un encodage adapté dans cette implémentation.
    
- **Valeurs manquantes** : doivent être traitées avant l'entraînement avec `GradientBoostingClassifier`.
    
- **Déséquilibre des classes** : le modèle ne rééquilibre pas automatiquement les observations. Évaluer des stratégies adaptées, notamment `sample_weight` ou le rééchantillonnage.
    
- **Métriques** : ne pas se limiter à l'accuracy ; examiner précision, rappel, F1, matrice de confusion et éventuellement PR-AUC.
    
- **Probabilités** : `predict_proba()` permet d'ajuster le seuil de décision selon le coût des erreurs.
    
- **Coût de calcul** : pour de grands datasets, `HistGradientBoostingClassifier` peut être nettement plus rapide.
    
- **Fuite de données** : effectuer les transformations apprises uniquement sur le train et à l'intérieur des folds de validation croisée.
    

**S'utilise souvent avec :** `GridSearchCV`, `RandomizedSearchCV`, `Pipeline`, `classification_report`, `permutation_importance`.