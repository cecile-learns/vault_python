# Boosting

## Quand l’utiliser ?

Combiner séquentiellement des modèles avec AdaBoost. Exemple du cours : reconnaissance de lettres.

## Principe

Le boosting combine de manière itérative des modèles faibles pour former un modèle plus robuste et plus puissant. AdaBosst est le plus populaire des algorithmes de boosting.


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
    X, y, test_size=0.3
)
```

## 3. Instancier, entraîner et prédire

### 3.1 Classification supervisée par arbre de décision

```python
from sklearn.tree import DecisionTreeClassifier

dtc=DecisionTreeClassifier(max_depth=5)
dtc.fit(X_train, y_train)

y_pred_dtc = dtc.predict(X_test)

```


### 3.2 AdaBoost 

```python
from sklearn.ensemble import AdaBoostClassifier


boosting = AdaBoostClassifier(estimator=DecisionTreeClassifier(max_depth=5),
                              n_estimators=400, learning_rate=1.0, random_state=42
boosting.fit(X_train, y_train)


#utilisation du résultat précédent possible
ac = AdaBoostClassifier(estimator=dtc, n_estimators=400) #base_estimator est l'ancienne version vue dans le cours 
ac.fit(X_train, y_train)


y_pred_boost = boosting.predict(X_test)
print("Accuracy :", boosting.score(X_test, y_test))
print(classification_report(y_test, y_pred_boost, zero_division=0))
```

# Évaluer le modèle

## Matrice de confusion

Lignes = classes réelles ; colonnes = classes prédites. La diagonale contient les bonnes classifications.

```python
y_pred =ac.predict(X_test)

pd.crosstab(y_test, y_pred, rownames=["Classe réelle"], colnames=["Classe prédite"])
```

## Accuracy et rapport par classe

Accuracy = proportion totale de bonnes réponses. La précision répond à « parmi les positifs prédits, combien sont corrects ? » ; le rappel à « parmi les positifs réels, combien sont retrouvés ? ». F1 combine précision et rappel ; support donne l’effectif réel de chaque classe.

```python
accuracy = ac.score(X_test, y_test)
print("Accuracy :", accuracy)
print(classification_report(y_test, y_pred, zero_division=0))
```

Une bonne accuracy peut masquer un mauvais rappel de la classe minoritaire. `zero_division=0` fixe à zéro une métrique indéfinie ; regarder la classe concernée et la cause.

## Paramètres utiles

- `n_estimators` : nombre de modèles ; pour AdaBoost, nombre maximal d’étapes.
- `learning_rate` : poids des contributions successives d’AdaBoost, à régler avec n_estimators.
- `random_state` : reproductibilité.

## Interprétation




## Points de vigilance

- Le cours utilise `base_estimator` : renommé `estimator` depuis sklearn 1.2 ; l’ancienne forme a été retirée en 1.4.
- AdaBoost peut être sensible au bruit et aux valeurs aberrantes ; réinsister sur les erreurs ne les rend pas automatiquement bien classées.
-
## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de classification.md`.

-
- [ensemble.AdaBoostClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.AdaBoostClassifier.html)

![[Pasted image 20261002092330.png]]