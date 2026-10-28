# Arbres de décision

## Quand l’utiliser ?

Obtenir des règles de décision non linéaires, lisibles si l’arbre reste peu profond. Exemple : diagnostic du cancer du sein.

## Principe

Des seuils successifs sur les variables divisent les observations en groupes de plus en plus homogènes.

> Application du cours : breast_cancer.csv. Les paramètres illustrent une première expérience, pas un optimum universel.

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

---

# Pas à pas

## 1. Charger les données et définir X / y

Retirer la cible des variables explicatives. Inspecter les valeurs manquantes ; la suppression de lignes ci-dessous reprend un choix simple des exercices, à réévaluer sur un autre jeu.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("breast_cancer.csv", index_col=0)
print(df.head())
print(df.isna().sum())
# Choix des exercices : supprimer les lignes incomplètes.
df = df.dropna().copy()
X = df.drop(columns=["diagnosis"])
y = df["diagnosis"]
```

## 2. Séparer train et test

`X` est une matrice 2D, `y` un vecteur 1D. Les quatre sorties restent alignées. Le train sert à apprendre ; le test reste à l’écart du choix des réglages. En classification, on peut ajouter `stratify=y` si les effectifs le permettent ; le découpage ci-dessous reprend celui du cours.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=123
)
print(X_train.shape, X_test.shape, y_train.shape, y_test.shape)
```

## 3. Fixer la représentation fournie aux modèles

Les arbres n’exigent pas de standardisation ; les variables doivent néanmoins être numériques.

```python
X_train_modele = X_train
X_test_modele = X_test
```

## 4. Instancier, entraîner et prédire

fit reçoit seulement le train et sa cible. predict reçoit le test dans la même représentation.

```python
from sklearn.tree import DecisionTreeClassifier

clf = DecisionTreeClassifier(criterion="entropy", max_depth=4, random_state=123)
clf.fit(X_train_modele, y_train)
y_pred = clf.predict(X_test_modele)
```

## Feature_importance

```python
feats = {}
for feature, importance in zip(data.columns, dt_clf.feature_importances_):
    feats[feature] = importance 
    
importances = pd.DataFrame.from_dict(feats, orient='index').rename(columns={0: 'Importance'})

#Affichage des 8 variables les plus importantes
importances.sort_values(by='Importance', ascending=False).head(8)
```

## Lire les règles et les variables utilisées

```python
from sklearn.tree import plot_tree

plt.figure(figsize=(18, 10))
plot_tree(clf, feature_names=list(X.columns), class_names=[str(c) for c in clf.classes_], filled=True)
plt.show()
importances = pd.Series(clf.feature_importances_, index=X.columns).sort_values(ascending=False)
print(importances)
```

Les importances sont des diminutions d’impureté, sans signe et sans interprétation causale.


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

## Paramètres utiles

- `criterion` : `"gini"` ou `"entropy"` pour choisir les divisions.
- `max_depth` : limite de profondeur ; None laisse l’arbre se développer selon les autres contraintes.
- `min_samples_leaf` : effectif minimal par feuille ; permet de limiter les règles trop particulières.
- `random_state` : reproductibilité des choix aléatoires.
- `filled=True` colore les nœuds ; `plot_tree` retourne des objets graphiques Matplotlib.

## Interprétation

`feature_importances_` mesure la contribution aux diminutions d’impureté. Cela ne mesure ni une causalité ni un effet signé.

# Pattern minimal

Les matrices ci-dessous ont déjà été préparées avec le **même objet appris sur le train** ; elles ne désignent pas les données brutes. 

```python
from sklearn.tree import DecisionTreeClassifier

modele = DecisionTreeClassifier(criterion="entropy", max_depth=4, random_state=123)
modele.fit(X_train_modele, y_train)
y_pred = modele.predict(X_test_modele)
```



## Points de vigilance

- Un arbre profond peut surapprendre.
- les arbres de décisions fonctionnent aussi bien avec des variables continues, discrètes ou qualitatives (après dichotomisation sur _scikit-learn_).
- Standardisation généralement inutile ; encoder les catégories.
- Les importances d’impureté peuvent favoriser les variables avec de nombreuses valeurs et se partager entre variables corrélées.


## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de classification.md`.

- [tree.DecisionTreeClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html)
- [tree.plot_tree](https://scikit-learn.org/stable/modules/generated/sklearn.tree.plot_tree.html)

![[Pasted image 20260930161632.png]]