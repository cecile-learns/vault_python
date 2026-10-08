# Random Forest

## Quand l’utiliser ?

Modéliser des relations non linéaires sur des données tabulaires. Exemple : prédiction du churn.

## Principe

Une forêt combine des arbres entraînés avec de l’aléa sur les observations et les variables. La diversification réduit la variance par rapport à un arbre isolé.

Random Forest = bagging d’arbres + sélection aléatoire de variables à chaque nœud + vote final.

> Application du cours : churn_dataset.csv. Les paramètres illustrent une première expérience, pas un optimum universel.

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

---

# Pas à pas

## 1. Charger les données et définir X / y

Retirer la cible des variables explicatives. Inspecter les valeurs manquantes ; la suppression de lignes ci-dessous reprend un choix simple des exercices, à réévaluer sur un autre jeu.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("churn_dataset.csv")

X = df.drop(columns=["churn"])
y = df["churn"]
```

## 2. Préparer les deux options catégorielles

Dans le cours : "il est préférable de remplacer ces features par des variables indicatrices, car les forêts aléatoires sur _scikit-learn_ ne gèrent pas les variables qualitatives"

```python
target = churn_df['churn']
#Exemple du cours
churn_df = churn_df.join(
    pd.get_dummies(
        churn_df['international plan'], prefix='international')
)
churn_df = churn_df.join(
    pd.get_dummies(
        churn_df['voice mail plan'], prefix='voicemail')
)

to_drop = ['international plan', 'voice mail plan', 'state', 'area code', 'phone number', 'churn']

data = churn_df.drop(to_drop,axis=1)
```

## 3. Séparer train et test

`X` est une matrice 2D, `y` un vecteur 1D. Les quatre sorties restent alignées. Le train sert à apprendre ; le test reste à l’écart du choix des réglages. En classification, on peut ajouter `stratify=y` si les effectifs le permettent ; le découpage ci-dessous reprend celui du cours.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=12
)
print(X_train.shape, X_test.shape, y_train.shape, y_test.shape)
```

## 4. Instancier, entraîner et prédire

fit reçoit seulement le train et sa cible. predict reçoit le test dans la même représentation.

```python
from sklearn.ensemble import RandomForestClassifier

clf = RandomForestClassifier(n_estimators=100, random_state=321, n_jobs=-1)
clf.fit(X_train, y_train)
y_pred = clf.predict(X_test)
```

# 5. Évaluer le modèle

## Matrice de confusion

Lignes = classes réelles ; colonnes = classes prédites. La diagonale contient les bonnes classifications.

```python
from sklearn.metrics import confusion_matrix, classification_report

cm = confusion_matrix(y_test, y_pred, labels=clf.classes_)
print(pd.DataFrame(cm, index=clf.classes_, columns=clf.classes_))

# Alternative Pandas : mêmes observations dans le même ordre.
pd.crosstab(y_test, y_pred, rownames=["Classe réelle"], colnames=["Classe prédite"])
```

## Accuracy et rapport par classe

Accuracy = proportion totale de bonnes réponses. La précision répond à « parmi les positifs prédits, combien sont corrects ? » ; le rappel à « parmi les positifs réels, combien sont retrouvés ? ». F1 combine précision et rappel ; support donne l’effectif réel de chaque classe.

```python
from sklearn.metrics import classification_report
accuracy = clf.score(X_test, y_test)
print("Accuracy :", accuracy)
print(classification_report(y_test, y_pred, zero_division=0))
```

Une bonne accuracy peut masquer un mauvais rappel de la classe minoritaire. `zero_division=0` fixe à zéro une métrique indéfinie ; regarder la classe concernée et la cause.

# 6. Probabilités et ciblage

Pour chaque observation, retrouver la probabilité de départ à partir de classes_. Le cours utilise ensuite une courbe de gains dans scikitplot, bibliothèque distincte de sklearn. (voir fiche lecture de courbe)

```python
#Probabilité pour X_test d'appartenir à chacun des deux classes (true ou false)
y_proba = clf.predict_proba(X_test)


table_probas = pd.DataFrame(y_proba, columns=clf.classes_, index=X_test.index)
print(table_probas.head())

importances = pd.Series(clf.feature_importances_, index=X.columns).sort_values(ascending=False)
print(importances.head(10))
```

Une probabilité de classe ne doit pas être confondue avec une importance de variable. Définir le label de départ avant tout classement pour une campagne.

## Paramètres utiles

- `n_estimators` : nombre d’arbres ; augmenter accroît le coût et stabilise souvent la prédiction.
- `max_depth`, `min_samples_leaf` : contrôlent la complexité de chaque arbre.
- `max_features` : nombre de variables candidates à chaque division ; `"sqrt"` est un choix courant en classification.
- `n_jobs=-1` : utilise tous les processeurs disponibles ; choisir 1 pour limiter les ressources.
- `random_state` : graine pour rendre l’expérience reproductible.
- `class_weight="balanced"` : pondération inverse des effectifs de classes ; à évaluer, pas une garantie de meilleurs résultats.

## Interprétation

Les probabilités suivent l’ordre de `clf.classes_`. Les importances décrivent l’usage des variables dans les divisions, pas une cause du résultat.

# Pattern minimal

Les matrices ci-dessous ont déjà été préparées avec le **même objet appris sur le train** ; elles ne désignent pas les données brutes. 

```python
from sklearn.ensemble import RandomForestClassifier

modele = RandomForestClassifier(n_estimators=100, random_state=321, n_jobs=-1)
modele.fit(X_train_modele, y_train)
y_pred = modele.predict(X_test_modele)
```

## Points de vigilance

- Une forêt peut surapprendre ; elle ne dispense pas d’évaluation indépendante.
- Les importances peuvent être biaisées par la cardinalité et les corrélations.
- La courbe de gains cumulés du cours utilise scikitplot, bibliothèque distincte de sklearn.

## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de classification.md`.

- [ensemble.RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)

**![[Pasted image 20261002142724.png]]**