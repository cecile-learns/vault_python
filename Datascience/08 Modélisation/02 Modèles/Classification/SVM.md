# SVM

## Quand l’utiliser ?


Classer des observations avec une frontière linéaire ou non linéaire. Exemple du cours : classification de vins.
Ne fonctionne qu'avec des variables numériques
## Principe

SVM recherche une séparation à grande marge. Le noyau permet de représenter des frontières non linéaires.

> Application du cours : wine.csv. Les paramètres illustrent une première expérience, pas un optimum universel.

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

---

# Pas à pas

## 1. Charger les données et définir X / y

Retirer la cible des variables explicatives. Inspecter les valeurs manquantes ; la suppression de lignes ci-dessous reprend un choix simple des exercices, à réévaluer sur un autre jeu.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("wine.csv")
print(df.head())
print(df.isna().sum())
# Choix des exercices : supprimer les lignes incomplètes.
df = df.dropna().copy()
X = df.drop(columns=["Wine"])
y = df["Wine"]
```

## 2. Séparer train et test

`X` est une matrice 2D, `y` un vecteur 1D. Les quatre sorties restent alignées. Le train sert à apprendre ; le test reste à l’écart du choix des réglages. En classification, on peut ajouter `stratify=y` si les effectifs le permettent ; le découpage ci-dessous reprend celui du cours.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=66
)
print(X_train.shape, X_test.shape, y_train.shape, y_test.shape)
```

## 3. Standardiser sans apprendre sur le test

Les distances ou la pénalisation dépendent des unités. `fit_transform` apprend les moyennes et écarts-types **sur le train**, puis le transforme. `transform` réutilise ces valeurs sur le test ; le test n’a pas à être exactement centré-réduit.

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_train_modele = scaler.fit_transform(X_train)
X_test_modele = scaler.transform(X_test)
```

## 4. Instancier, entraîner et prédire

fit reçoit seulement le train et sa cible. predict reçoit le test dans la même représentation.

```python
from sklearn.svm import SVC

clf = SVC(C=1.0, kernel="poly", gamma=0.01)
clf.fit(X_train_modele, y_train)
y_pred = clf.predict(X_test_modele)
```


# Évaluer le modèle

## Matrice de confusion

Lignes = classes réelles ; colonnes = classes prédites. La diagonale contient les bonnes classifications.

```python
from sklearn.metrics import confusion_matrix, classification_report

cm = confusion_matrix(y_test, y_pred, labels=clf.classes_)
print(pd.DataFrame(cm, index=clf.classes_, columns=clf.classes_))
# Alternative Pandas : mêmes observations dans le même ordre.
print(pd.crosstab(y_test, y_pred, rownames=["Classe réelle"], colnames=["Classe prédite"]))
```

## Accuracy et rapport par classe

Accuracy = proportion totale de bonnes réponses. La précision répond à « parmi les positifs prédits, combien sont corrects ? » ; le rappel à « parmi les positifs réels, combien sont retrouvés ? ». F1 combine précision et rappel ; support donne l’effectif réel de chaque classe.

```python
accuracy = clf.score(X_test_modele, y_test)
print("Accuracy :", accuracy)
print(classification_report(y_test, y_pred, zero_division=0))
```

Une bonne accuracy peut masquer un mauvais rappel de la classe minoritaire. `zero_division=0` fixe à zéro une métrique indéfinie ; regarder la classe concernée et la cause.

## Quand régler C, gamma et le noyau ?

Utiliser une validation **dans le train** pour comparer les réglages. Ne pas choisir le noyau à partir du résultat du test. Le scaler doit être réappris dans chaque pli ; la fiche GridSearchCV rappelle cette distinction. `predict_proba` n’est disponible que si probability=True a été demandé avant fit ; cela n’est pas nécessaire pour la matrice de confusion.

## Paramètres utiles

- `C` : compromis entre marge et erreurs d’entraînement ; grand C = erreurs davantage pénalisées.
- `kernel` : `"linear"`, `"rbf"` ou `"poly"` ; le cours illustre le noyau polynomial.
- `gamma` : portée de l’influence des observations pour les noyaux rbf/poly ; `"scale"` fournit un réglage automatique.
- `degree` : degré du noyau polynomial, sans effet sur rbf/linear.
- `probability=True` doit être défini avant fit pour disposer de predict_proba ; cela augmente le coût du calcul.

## Interprétation

`predict` renvoie des classes. Un bon score d’entraînement seul ne démontre pas une bonne généralisation.

# Pattern minimal

Les matrices ci-dessous ont déjà été préparées avec le **même objet appris sur le train** ; elles ne désignent pas les données brutes. 

```python
from sklearn.svm import SVC

modele = SVC(C=1.0, kernel="poly", gamma=0.01)
modele.fit(X_train_modele, y_train)
y_pred = modele.predict(X_test_modele)
```



## Points de vigilance

- Standardisation importante pour les distances et la régularisation.
- Ajuster C, gamma et le noyau par validation croisée sur le train ; voir GridSearchCV.
- Les SVM à noyau peuvent devenir coûteux sur un grand nombre d’observations.


## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de classification.md`.

- [svm.SVC](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html)

