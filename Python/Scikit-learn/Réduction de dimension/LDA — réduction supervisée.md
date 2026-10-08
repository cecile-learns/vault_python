# LDA — réduction supervisée

## Quand l’utiliser ?

Projeter les données en utilisant les classes connues ; exemple de comparaison avec PCA sur Fashion-MNIST.

## Principe

LinearDiscriminantAnalysis recherche une projection qui sépare les classes relativement à leur dispersion interne. Il s’agit ici d’une réduction supervisée.

> Cours : Fashion-MNIST, comparaison d’une forêt avant et après réduction. Les fichiers train/test sont déjà séparés.

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

---

# Pas à pas

## 1. Charger les jeux et isoler la cible

Les pixels sont numériques et dans une même unité. Ne pas inclure label dans la projection. Ne pas refusionner les fichiers pour apprendre la réduction.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.ensemble import RandomForestClassifier

train = pd.read_csv("fashion_mnist_train.csv")
test = pd.read_csv("fashion_mnist_test.csv")
X_train = train.drop(columns=["label"])
y_train = train["label"]
X_test = test.drop(columns=["label"])
y_test = test["label"]
assert X_train.columns.equals(X_test.columns)
assert X_train.notna().all().all() and X_test.notna().all().all()
```

## 2. Établir une référence sans réduction

La référence utilise les mêmes jeux. La comparaison illustre l’effet de la représentation ; pour sélectionner systématiquement les réglages, utiliser la validation interne au train.

```python
reference = RandomForestClassifier(n_estimators=100, random_state=42, n_jobs=-1)
reference.fit(X_train, y_train)
accuracy_reference = reference.score(X_test, y_test)
```

## 3. Apprendre la réduction sur le train

LDA est supervisée : y_train est nécessaire pour apprendre la séparation. La projection du test utilise uniquement l’objet appris, jamais y_test.

```python
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis

reducteur = LinearDiscriminantAnalysis()
X_train_reduit = reducteur.fit_transform(X_train, y_train)
X_test_reduit = reducteur.transform(X_test)
print(X_train.shape, X_train_reduit.shape)
```

Le nombre de dimensions est au plus min(nombre de variables, nombre de classes − 1). Avec deux classes, il n’y a au plus qu’un axe discriminant.

## 4. Entraîner le modèle sur la représentation réduite

Utiliser la même transformation pour le test et conserver la correspondance avec les cibles.

```python
clf = RandomForestClassifier(n_estimators=100, random_state=42, n_jobs=-1)
clf.fit(X_train_reduit, y_train)
y_pred = clf.predict(X_test_reduit)
print("Accuracy sans réduction :", accuracy_reference)
print("Accuracy avec réduction :", clf.score(X_test_reduit, y_test))
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
accuracy = clf.score(X_test_reduit, y_test)
print("Accuracy :", accuracy)
print(classification_report(y_test, y_pred, zero_division=0))
```

Une bonne accuracy peut masquer un mauvais rappel de la classe minoritaire. `zero_division=0` fixe à zéro une métrique indéfinie ; regarder la classe concernée et la cause.

## Paramètres utiles

- `y_train` : étiquettes de classification, indispensables à l’apprentissage.
- `n_components` : au maximum min(nombre de variables, nombre de classes − 1) ; None conserve les dimensions disponibles.
- `solver="svd"` : solveur par défaut ; `"lsqr"` ne permet pas transform.

## Interprétation

Avec deux classes, la projection discriminante a au plus un axe. LDA ne doit pas recevoir une cible de régression continue.

# Pattern minimal

Pour la réduction seule, après préparation des jeux :

```python
reducteur = LinearDiscriminantAnalysis()
X_train_reduit = reducteur.fit_transform(X_train, y_train)
X_test_reduit = reducteur.transform(X_test)
```



## Points de vigilance

- Ne pas apprendre avec y_test.
- Ne pas confondre LinearDiscriminantAnalysis avec LatentDirichletAllocation, également abrégé LDA dans d’autres contextes.
- La structure des classes et les hypothèses de covariance influencent la pertinence de la projection.


## Références

Source de formation : `01_RESSOURCES/Liora/Réduction de dimension.md`.

- [discriminant_analysis.LinearDiscriminantAnalysis](https://scikit-learn.org/stable/modules/generated/sklearn.discriminant_analysis.LinearDiscriminantAnalysis.html)

