# KNN

## Quand l’utiliser ?

Classer par proximité avec des observations connues. Exemple : reconnaissance des chiffres avec load_digits.

## Principe

Le modèle prédit à partir des k voisins les plus proches ; le choix de la distance et de l’échelle des variables est déterminant.

> Application du cours : chiffres manuscrits de load_digits. Les paramètres illustrent une première expérience, pas un optimum universel.

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

---

# Pas à pas

## 1. Charger les données et définir X / y

Retirer la cible des variables explicatives. Inspecter les valeurs manquantes ; la suppression de lignes ci-dessous reprend un choix simple des exercices, à réévaluer sur un autre jeu.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import load_digits

digits = load_digits()
X = pd.DataFrame(digits.data)
y = digits.target
print(X.shape, y.shape)
```

## 2. Séparer train et test

`X` est une matrice 2D, `y` un vecteur 1D. Les quatre sorties restent alignées. Le train sert à apprendre ; le test reste à l’écart du choix des réglages. En classification, on peut ajouter `stratify=y` si les effectifs le permettent ; le découpage ci-dessous reprend celui du cours.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=126
)
print(X_train.shape, X_test.shape, y_train.shape, y_test.shape)
```

## 3. Fixer la représentation fournie aux modèles

Les pixels ont la même unité dans load_digits : l’exemple peut utiliser leurs valeurs directement. Pour des variables hétérogènes, apprendre un StandardScaler sur le train.

```python
X_train_modele = X_train
X_test_modele = X_test
```

## 4. Instancier, entraîner et prédire

fit reçoit seulement le train et sa cible. predict reçoit le test dans la même représentation.

```python
from sklearn.neighbors import KNeighborsClassifier

knn = neighbors.KNeighborsClassifier(n_neighbors=7, metric='minkowski')
knn.fit(X_train, y_train)

y_pred = knn.predict(X_test_modele)
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
accuracy = knn.score(X_test_modele, y_test)
print("Accuracy :", accuracy)
print(classification_report(y_test, y_pred, zero_division=0))
```

Une bonne accuracy peut masquer un mauvais rappel de la classe minoritaire. `zero_division=0` fixe à zéro une métrique indéfinie ; regarder la classe concernée et la cause.

## Paramètres utiles

- `n_neighbors` : nombre de voisins ; ne doit pas dépasser le nombre d’observations du train, y compris dans chaque pli de CV.
- `metric="minkowski", p=2` : distance euclidienne ; `p=1` correspond à Manhattan.
- `metric="manhattan"` et `"chebyshev"` sont aussi comparées dans le cours.
- `weights="uniform"` : vote égal ; `"distance"` : poids plus élevé pour les voisins proches.

## Interprétation

Petit k : frontière plus locale, sensible au bruit. Grand k : décision plus lissée, pouvant perdre des distinctions utiles.

# Pattern minimal

Les matrices ci-dessous ont déjà été préparées avec le **même objet appris sur le train** ; elles ne désignent pas les données brutes. 

```python
from sklearn.neighbors import KNeighborsClassifier

modele = KNeighborsClassifier(n_neighbors=7, metric="minkowski", p=2)
modele.fit(X_train_modele, y_train)
y_pred = modele.predict(X_test_modele)
```



## Points de vigilance

- Comparer plusieurs k sur la validation ; garder le test final indépendant.
- Standardiser si les unités diffèrent ; les pixels du cours ont déjà une échelle commune.
- En grande dimension, la notion de proximité devient moins discriminante ; prédire peut coûter cher.


## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de classification.md`.

- [neighbors.KNeighborsClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html)

```Python
score_miniko=[]
score_man=[]
score_cheb=[]

score_minko = []
score_man = []
score_cheb = []

for k in range(1, 41):
    knn = neighbors.KNeighborsClassifier(n_neighbors=k)
    knn.fit(X_train, y_train)
    score_minko.append(knn.score(X_test, y_test))

for k in range(1, 41):
    knn = neighbors.KNeighborsClassifier(n_neighbors=k, metric='manhattan')
    knn.fit(X_train, y_train)
    score_man.append(knn.score(X_test, y_test))
    
for k in range(1, 41):
    knn = neighbors.KNeighborsClassifier(n_neighbors=k, metric='chebyshev')
    knn.fit(X_train, y_train)
    score_cheb.append(knn.score(X_test, y_test))


import matplotlib.pyplot as plt
%matplotlib inline

plt.plot(range(1, 41), score_minko, color='blue', label='Minkowski')
plt.plot(range(1, 41), score_man, color='green', label='Manhattan')
plt.plot(range(1, 41), score_cheb, color='gray', label='Chebyshev')
plt.title('Score - valeur de K')  
plt.xlabel('Valeur de K')  
plt.ylabel('Accuracy') 
plt.legend();

```
![[Pasted image 20260930154517.png]]