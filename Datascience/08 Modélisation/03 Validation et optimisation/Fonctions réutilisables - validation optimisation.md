
# Quand l’utiliser ?

Après avoir construit un premier modèle, pour :

- estimer sa performance de façon plus robuste avec la validation croisée ;
- tenir compte d’un éventuel déséquilibre de classes ;
- comparer plusieurs stratégies de rééchantillonnage ;
- ajuster le seuil de classification ;
- optimiser les hyperparamètres.

Principe général :

```
modèle de base
→ validation croisée
→ diagnostic
→ optimisation ciblée
→ nouvelle validation
```

L’optimisation doit porter sur un problème identifié et sur une **métrique cohérente avec l’objectif métier**. 

# 1. Validation croisée stratifiée

## Quand l’utiliser ?

Pour évaluer un modèle de classification sur plusieurs découpages tout en conservant approximativement la proportion des classes dans chaque fold.

## Pattern

```python
from sklearn.model_selection import StratifiedKFold, cross_validate

cv = StratifiedKFold(
    n_splits=5,    
    shuffle=True,    
    random_state=42)
    
scores = cross_validate(
    model,    
    X,    
    y,    
    cv=cv,    
    scoring=[
            "accuracy",        
            "precision",        
            "recall",        
            "f1"    ])
```


## Sorties utiles

```python
scores["test_accuracy"]
scores["test_precision"]
scores["test_recall"]
scores["test_f1"]
```

Moyenne :

```python
scores["test_f1"].mean()
```

### À retenir

```
StratifiedKFold
→ conserve la proportion des classes

cross_validate
→ peut calculer plusieurs métriques simultanément
```

---

# 2. `crossvalidation_resample()`

## Quand l’utiliser ?

Pour comparer plusieurs méthodes de rééquilibrage de classes **sans contaminer les folds de validation**.

Le point essentiel est que le rééchantillonnage doit être appliqué **uniquement au sous-échantillon d’entraînement de chaque fold**. Texte collé (3)

## Pattern du cours, nettoyé

```python
from sklearn.model_selection import StratifiedKFold
from sklearn.metrics import f1_score
from imblearn.over_sampling import (SMOTE, RandomOverSampler)
from imblearn.under_sampling import RandomUnderSampler
import numpy as np

def crossvalidation_resample(X, y, model):
    resamplers = {
            "SMOTE": SMOTE(),        
            "Oversampling": RandomOverSampler(            
            sampling_strategy="not majority"        
            ),        
            "Undersampling": RandomUnderSampler(
                sampling_strategy="majority"        
            )    
        }    
	cv = StratifiedKFold(
	        n_splits=5,        
	        shuffle=True,        
	        random_state=42    
	        )    
	
	for name, resampler in resamplers.items():        
	    f1_scores = []        
	    for train_index, val_index in cv.split(X, y):
	        X_train = X.iloc[train_index]
            y_train = y.iloc[train_index]

            X_val = X.iloc[val_index]
            y_val = y.iloc[val_index]

            X_resampled, y_resampled = (
                resampler.fit_resample(
                    X_train,
                    y_train
                )
            )

            model.fit(
                X_resampled,
                y_resampled
            )

            y_pred = model.predict(X_val)

            f1_scores.append(
                f1_score(y_val, y_pred)
            )

        print(name)
        print(
            "F1 moyen :",
            np.mean(f1_scores)
        )
```

La fonction originale compare SMOTE, oversampling et undersampling sur cinq folds. 

## Logique interne

```
fold
→ sous-train
→ rééchantillonnage
→ fit du modèle
→ prédiction sur validation non rééchantillonnée
→ F1
```

### Vigilance

Ne jamais faire :

```python
X_resampled, y_resampled = SMOTE().fit_resample(X, y)
cross_validate(model, X_resampled, y_resampled)
```

car les observations synthétiques seraient créées **avant** la séparation des folds.

---

# 3. Transformer une probabilité en classe selon un seuil

## Quand l’utiliser ?

Quand le seuil `0.5` par défaut ne correspond pas au meilleur compromis métier ou à la métrique choisie.

## Fonction du cours

```python
def score(probabilite, seuil):
    return (probabilite >= seuil).astype("int")
```

Je la renommerais pour le Vault :

```python
def proba_to_class(probabilities, threshold=0.5):
    return (        
    probabilities >= threshold    
    ).astype(int)
```

Usage :

```python
y_prob = model.predict_proba(X)[:, 1]
y_pred = proba_to_class(
    y_prob,    
    threshold=0.67)
```

## À retenir

```
predict_proba()
→ probabilité

seuil
→ règle de décision

classe
→ 0 ou 1
```

Changer le seuil **ne réentraîne pas le modèle** : on change seulement la règle permettant de convertir une probabilité en classe.

---

# 4. `crossvalidation_resample_threshold()`

## Quand l’utiliser ?

Quand on veut combiner :

- validation croisée ;
- rééchantillonnage ;
- recherche d’un seuil optimal ;
- optimisation d’une métrique.

C’est le pattern le plus complexe du module. 

## Pattern nettoyé

```python
def crossvalidation_resample_threshold(
    X,
    y,
    model,
    thresholds=None
):

    if thresholds is None:
        thresholds = np.arange(
            0,
            1,
            0.01
        )

    resamplers = {
        "SMOTE": SMOTE(),
        "Oversampling": RandomOverSampler(
            sampling_strategy="not majority"
        ),
        "Undersampling": RandomUnderSampler(
            sampling_strategy="majority"
        )
    }

    cv = StratifiedKFold(
        n_splits=5,
        shuffle=True,
        random_state=42
    )

    for name, resampler in resamplers.items():

        best_thresholds = []
        best_scores = []

        for train_index, val_index in cv.split(X, y):

            X_train = X.iloc[train_index]
            y_train = y.iloc[train_index]

            X_val = X.iloc[val_index]
            y_val = y.iloc[val_index]

            X_resampled, y_resampled = (
                resampler.fit_resample(
                    X_train,
                    y_train
                )
            )

            model.fit(
                X_resampled,
                y_resampled
            )

            y_prob = model.predict_proba(
                X_val
            )[:, 1]

            scores = [
                f1_score(
                    y_val,
                    proba_to_class(
                        y_prob,
                        threshold
                    )
                )
                for threshold in thresholds
            ]

            best_index = np.argmax(scores)

            best_thresholds.append(
                thresholds[best_index]
            )

            best_scores.append(
                scores[best_index]
            )

        print(name)

        print(
            "Seuil moyen :",
            np.mean(best_thresholds)
        )

        print(
            "F1 moyen :",
            np.mean(best_scores)
        )
```

### Modification volontaire

Le cours teste :

```
np.arange(0, 1, 0.001)
```

soit environ **1000 seuils par fold**. Texte collé (3)

Pour une exploration plus rapide :

```
np.arange(0, 1, 0.01)
```

→ environ 100 seuils.

On peut ensuite raffiner autour de la zone intéressante.

### Vigilance

Cette fonction est à considérer comme :

> **Pattern avancé à copier, comprendre et adapter — pas à réécrire de mémoire.**

---

# 5. Appliquer la stratégie retenue au train complet

Après la validation croisée, on choisit par exemple :

```
SMOTE
+
seuil 0.68
```

On réentraîne alors le modèle sur **tout le train rééchantillonné**, puis on applique le seuil choisi au test.

Pattern du cours : Texte collé (3)

```python
smote = SMOTE()

X_train_resampled, y_train_resampled = (
    smote.fit_resample(
        X_train,
        y_train
    )
)

model.fit(
    X_train_resampled,
    y_train_resampled
)

y_prob = model.predict_proba(
    X_test
)[:, 1]

y_pred = proba_to_class(
    y_prob,
    threshold=0.68
)
```

### Distinction importante

```
Validation
→ choisir resampling + seuil

Train complet
→ réentraîner avec les choix retenus

Test
→ mesurer seulement la performance finale
```

---

# 6. `GridSearchCV`

## Quand l’utiliser ?

Pour comparer automatiquement plusieurs combinaisons d’hyperparamètres grâce à la validation croisée.

Pattern :

```python
from sklearn.model_selection import GridSearchCV

param_grid = {
    "max_depth": [3, 5, 10],
    "criterion": [
        "gini",
        "entropy"
    ]
}

grid = GridSearchCV(
    estimator=model,
    param_grid=param_grid,
    scoring="recall",
    cv=5
)

grid.fit(
    X_train,
    y_train
)
```

Résultats :

```
grid.best_params_
grid.best_score_
grid.best_estimator_
```

Puis :

```
y_pred = grid.predict(X_test)
```

## Principe

```
estimateur
+
grille de valeurs
+
métrique
+
cross-validation
=
meilleur réglage parmi ceux testés
```

### Vigilances

`GridSearchCV` ne trouve pas :

> « le meilleur modèle possible ».

Il trouve :

> **la meilleure combinaison parmi celles qu’on lui a demandé de tester, selon la métrique choisie.**

Et le cours de méthodologie rappelle que ce genre d’optimisation peut être très coûteux en temps de calcul. Texte collé (3)

---

# Comment choisir entre ces outils ?

|Besoin|Outil|
|---|---|
|Évaluer sur plusieurs folds|`cross_validate()`|
|Respecter le déséquilibre des folds|`StratifiedKFold`|
|Comparer SMOTE / over / under|`crossvalidation_resample()`|
|Modifier la règle de décision|`proba_to_class()`|
|Chercher resampling + seuil|`crossvalidation_resample_threshold()`|
|Optimiser les hyperparamètres|`GridSearchCV`|

## Repère Vault

```
Validation - Optimisation
├── Cross-validation
├── StratifiedKFold
├── Rééchantillonnage dans la CV
├── Seuil de classification
├── CV + resampling + seuil
└── GridSearchCV
```

La fonction à vraiment **archiver telle une boîte à outils** est `crossvalidation_resample_threshold()` : c’est précisément le morceau de code qu’il serait peu rentable de reconstruire à chaque projet. Les autres sont surtout des patterns courts à retrouver rapidement.