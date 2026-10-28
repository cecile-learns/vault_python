# Stacking

## Quand l’utiliser ?

Combiner plusieurs familles de classifieurs. Exemple du cours : KNN, forêt et régression logistique sur le diabète.

## Principe
Le **Stacking** est une méthode d'ensemble dont le principe est l'entraînement simultané de divers algorithmes de Machine Learning dont les résultats sont ensuite utilisés pour entraîner un nouveau modèle qui permet de combiner de manière optimale les prédictions des premiers estimateurs.  
Cette méthode repose sur la technique suivante :

> - La spécification d'une liste de L algorithmes de base et des paramètres correspondant, ainsi que de l'algorithme de **meta-apprentissage**.
> - Chacun des L algorithmes est ensuite entraîné sur l'ensemble d'entraînement, contenant N observations.
>- Une procédure de validation croisée permet d'obtenir les prédictions de chacun des modèles pour les N observations.
> - L'algorithme de **méta-apprentissage** est ensuite entraîné sur ces données collectées et effectue de nouvelles prédictions.

Le modèle d'ensemble consiste finalement en l'ensemble des L algorithmes de base et du modèle de méta-apprentissage et peut être utilisé pour générer des prédictions sur un nouveau jeu de données.


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
clf = StackingClassifier(estimators=estimateurs, final_estimator=clf3)

clf.fit(X_train, y_train)
y_pred = clf.predict(X_test)

```

# 4. Évaluer le modèle

## Matrice de confusion

Lignes = classes réelles ; colonnes = classes prédites. La diagonale contient les bonnes classifications.

```python
from sklearn.metrics import confusion_matrix, classification_report

pd.crosstab(y_test, y_pred, rownames=["Classe réelle"], colnames=["Classe prédite"])
```

## Accuracy et rapport par classe

Accuracy = proportion totale de bonnes réponses. La précision répond à « parmi les positifs prédits, combien sont corrects ? » ; le rappel à « parmi les positifs réels, combien sont retrouvés ? ». F1 combine précision et rappel ; support donne l’effectif réel de chaque classe.

```python

scores = cross_validate(sclf, X_train, y_train, cv=cv3, scoring=['accuracy', 'f1'])

print("[StackingClassifier]: \n Accuracy: %0.2f (+/- %0.2f)\n" % (scores['test_accuracy'].mean(), scores['test_accuracy'].std()),
      "F1 score: %0.2f (+/- %0.2f)" % (scores['test_f1'].mean(), scores['test_f1'].std()))
```

Une bonne accuracy peut masquer un mauvais rappel de la classe minoritaire. `zero_division=0` fixe à zéro une métrique indéfinie ; regarder la classe concernée et la cause.


## Paramètres utiles

- `estimators` : liste de couples `(nom_unique, modèle)` ; les noms servent aussi à régler `knn__n_neighbors`, par exemple.
- `final_estimator` : modèle qui apprend à combiner les sorties.
- `cv` dans StackingClassifier : plis servant aux prédictions d’apprentissage du modèle final, pas une évaluation finale de l’ensemble.

## Interprétation

Comparer l’ensemble à ses modèles de base : combiner plusieurs modèles ne garantit pas un gain.

## Points de vigilance

- Le vote soft est pertinent si les probabilités sont convenablement calibrées.
- Les différents modèles peuvent demander des prétraitements différents. Le code reprend l’assemblage du cours ; les pipelines par modèle restent à traiter plus tard.
- Pour une comparaison rigoureuse, réapprendre les prétraitements au sein des plis, y compris ceux du stacking.
- Ne pas entraîner le modèle final sur des prédictions faites par des modèles ayant déjà appris les mêmes observations.


## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de classification.md`.

- [ensemble.VotingClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.VotingClassifier.html)
- [ensemble.StackingClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.StackingClassifier.html)

![[Pasted image 20261002160934.png]]