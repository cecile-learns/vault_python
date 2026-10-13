# Voting

## Quand l’utiliser ?

Combiner plusieurs familles de classifieurs. Exemple du cours : KNN, forêt et régression logistique sur le diabète.

## Principe

Voting combine plusieurs estimateurs de ML, similaires ou conceptuellement différents. C'est comme constitué un collège d'experts (arbre de décision, knn, etc), chacun va voter. 
- Dans le vote 'hard', chaque modèle de classification prédit un label, et le label final produit est celui prédit le plus fréquemment.
    
- Dans le vote 'soft', chaque modèle retourne une probabilité pour chaque classe, et la moyenne des probabilités est calculée pour prédire la classe finale (seulement recommandé si les classifieurs sont bien calibrés).

Dans les deux cas, il est possible d'affecter un poids à chaque estimateur, permettant de donner plus d'importance à un ou plusieurs modèles.

---

# Pas à pas

## 1. Charger les données et définir X / y

Retirer la cible des variables explicatives. Inspecter les valeurs manquantes ; la suppression de lignes ci-dessous reprend un choix simple des exercices, à réévaluer sur un autre jeu.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("diabetes.csv")

X = df.drop(columns=["Outcome"])
y = df["Outcome"]
```

## 2. Séparer train et test

`X` est une matrice 2D, `y` un vecteur 1D. Les quatre sorties restent alignées. Le train sert à apprendre ; le test reste à l’écart du choix des réglages. En classification, on peut ajouter `stratify=y` si les effectifs le permettent ; le découpage ci-dessous reprend celui du cours.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=4
)

```

## 3. Instancier, entraîner et prédire

fit reçoit seulement le train et sa cible. predict reçoit le test dans la même représentation.

```python
from sklearn.ensemble import VotingClassifier, StackingClassifier, RandomForestClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.linear_model import LogisticRegression

estimateurs = [
    ("knn", KNeighborsClassifier(n_neighbors=3)),
    ("rf", RandomForestClassifier(random_state=123)),
    ("lr", LogisticRegression(max_iter=1000)),
]
clf = VotingClassifier(estimators=estimateurs, voting="hard")

clf.fit(X_train_modele, y_train)

y_pred = clf.predict(X_test_modele)
```

# Évaluer le modèle


## Comparer sur les plis du train, Accuracy et F1 score

```python
from sklearn.model_selection import KFold, cross_validate

plis = KFold(n_splits=3, shuffle=True, random_state=111)
for nom, modele in estimateurs + [("voting", clf), ("stacking", stacking)]:
    scores = cross_validate(modele, X_train_modele, y_train, cv=plis,
                            scoring=["accuracy", "f1"])
    print(nom, scores["test_accuracy"].mean(), scores["test_f1"].mean())
```

dans le cours : 
[KNN]: 
 Accuracy: 0.71 (+/- 0.02) F1 score: 0.58 (+/- 0.02)
[Random Forest]: 
 Accuracy: 0.76 (+/- 0.03) F1 score: 0.63 (+/- 0.04)
[Logistic Regression]: 
 Accuracy: 0.76 (+/- 0.02) F1 score: 0.61 (+/- 0.05)
[Voting Classifier]: 
 Accuracy: 0.77 (+/- 0.00) F1 score: 0.64 (+/- 0.02)

## Paramètres utiles

- `estimators` : liste de couples `(nom_unique, modèle)` ; les noms servent aussi à régler `knn__n_neighbors`, par exemple.
- `voting="hard"` : majorité des classes ; `"soft"` : moyenne des probabilités, nécessite predict_proba chez tous les modèles.
- `weights` dans VotingClassifier : poids de chaque modèle, dans le même ordre que la liste.
- `final_estimator` : modèle qui apprend à combiner les sorties.
- `cv` dans StackingClassifier : plis servant aux prédictions d’apprentissage du modèle final, pas une évaluation finale de l’ensemble.

## Interprétation

Comparer l’ensemble à ses modèles de base : combiner plusieurs modèles ne garantit pas un gain.


## Points de vigilance

- Le vote soft est pertinent si les probabilités sont convenablement calibrées.
- Les différents modèles peuvent demander des prétraitements différents. Le code reprend l’assemblage du cours ; les pipelines par modèle restent à traiter plus tard.
- Pour une comparaison rigoureuse, réapprendre les prétraitements au sein des plis
- Ne pas entraîner le modèle final sur des prédictions faites par des modèles ayant déjà appris les mêmes observations.

Comme tout modèle de _scikit-learn_, il est également possible d'appliquer la fonction `GridSearchCV()` au meta-classifieur, pour sélectionner les hyper-paramètres de chaque sous-modèle.  
Il est également possible d'effectuer un _grid search_ sur l'ensemble des arguments du paramètre _estimators_, pour sélectionner les modèles donnant les meilleurs résultats.
## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de classification.md`.

- [ensemble.VotingClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.VotingClassifier.html)
- [ensemble.StackingClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.StackingClassifier.html)

![[Pasted image 20261002160920.png]]