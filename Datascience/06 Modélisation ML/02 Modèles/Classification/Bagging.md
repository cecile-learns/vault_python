# Bagging

## Quand l’utiliser ?

Stabiliser des arbres avec le bagging . 
Exemple du cours : reconnaissance de lettres.

## Principe

"Les méthodes de _bagging_ et de _boosting_ sont des "méta-algorithmes" dont l'approche est de combiner plusieurs algorithmes d'apprentissage automatique en un modèle prédictif, afin de réduire leur variance ou leur biais, et d'améliorer la performance finale."

Le bagging agrège des modèles entraînés sur des rééchantillonnages. La méthode construit un ensemble d'estimateurs indépendants qui sont par la suite agrégés en un méta-modèle.


> Application du cours : letter-recognition.csv. Les paramètres illustrent une première expérience, pas un optimum universel.


---

# Pas à pas

## 1. Charger les données et définir X / y

Retirer la cible des variables explicatives. Inspecter les valeurs manquantes ; la suppression de lignes ci-dessous reprend un choix simple des exercices, à réévaluer sur un autre jeu.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("letter-recognition.csv")
print(df.head())
print(df.isna().sum())
# Choix des exercices : supprimer les lignes incomplètes.
df = df.dropna().copy()
X = df.drop(columns=["letter"], axis=1)
y = df["letter"]
```

## 2. Séparer train et test

`X` est une matrice 2D, `y` un vecteur 1D. Les quatre sorties restent alignées. Le train sert à apprendre ; le test reste à l’écart du choix des réglages. En classification, on peut ajouter `stratify=y` si les effectifs le permettent ; le découpage ci-dessous reprend celui du cours.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42
)

```

## 3. Instancier, entraîner et prédire


```python
from sklearn.ensemble import BaggingClassifier

bc = BaggingClassifier(n_estimators=1000, oob_score=True)
bc.fit(X_train, y_train)
y_pred = bc.predict(X_test_modele)
```

# Évaluer le modèle

## Matrice de confusion

Lignes = classes réelles ; colonnes = classes prédites. La diagonale contient les bonnes classifications.

```python
y_pred = bc.predict(X_test)

pd.crosstab(y_test, y_pred, rownames=["Classe réelle"], colnames=["Classe prédite"])
```

## Accuracy et rapport par classe

Accuracy = proportion totale de bonnes réponses. La précision répond à « parmi les positifs prédits, combien sont corrects ? » ; le rappel à « parmi les positifs réels, combien sont retrouvés ? ». F1 combine précision et rappel ; support donne l’effectif réel de chaque classe.

```python
accuracy = bc.score(X_test, y_test)
print("Accuracy :", accuracy)
print(classification_report(y_test, y_pred, zero_division=0))
```

Une bonne accuracy peut masquer un mauvais rappel de la classe minoritaire. `zero_division=0` fixe à zéro une métrique indéfinie ; regarder la classe concernée et la cause.

## Score OOB du bagging

```python
bc.oob_score_
```

Chaque prédiction OOB agrège les modèles qui n’ont pas vu l’observation dans leur bootstrap. Ce contrôle ne remplace pas toutes les précautions de validation.


## Paramètres utiles

- `oob_score=True` : estimation à partir des observations absentes de chaque échantillon bootstrap ; nécessite `bootstrap=True` (valeur par défaut).
- `random_state` : reproductibilité.

## Interprétation

`oob_score_` est ici une accuracy, pas un taux d’erreur : erreur OOB = 1 − accuracy OOB. Les deux modèles retournent des classes.


## Points de vigilance

- L’OOB ne protège pas de toutes les fuites de données et ne garantit pas l’absence de surapprentissage.
- XGBoost, également abordé dans le cours, relève du package xgboost.


## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de classification.md`.

- [ensemble.BaggingClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.BaggingClassifier.html)
- [ensemble.AdaBoostClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.AdaBoostClassifier.html)

![[Pasted image 20261002092350.png]]