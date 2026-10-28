# XGBoost

## Quand l’utiliser ?

XGBoost est une bibliothèque de Machine Learning distincte de `scikit-learn`. Elle implémente notamment le **gradient boosting d’arbres** pour la classification et la régression.

Elle est adaptée aux données tabulaires lorsque les relations sont complexes et non linéaires. Son coût de calcul et son nombre de réglages imposent toutefois une validation rigoureuse.

> Application du cours : prédire si le revenu d’un individu dépasse 50 000 dollars à partir du jeu `adult.csv`.

---

## Principe

Les arbres sont construits **séquentiellement**. Chaque nouvel arbre cherche à corriger les erreurs de l’ensemble déjà construit en réduisant la fonction de perte.

```text
premier arbre
↓
analyse des erreurs
↓
ajout d’un arbre correctif
↓
répétition jusqu’au nombre d’itérations choisi
```

Le `learning_rate` réduit la contribution de chaque nouvel arbre :

```text
learning_rate plus faible
→ corrections plus petites
→ davantage d’arbres généralement nécessaires
→ apprentissage plus lent et plus progressif
```

Un `learning_rate` faible ne garantit pas à lui seul un meilleur modèle. Il doit être associé au nombre d’itérations et contrôlé sur un jeu de validation.

![[Pasted image 20261002163508.png]]

Les blocs du déroulé s’exécutent **dans l’ordre**, dans le même notebook. Le pattern minimal final suppose que les données sont déjà préparées.

---

# Pas à pas

## 1. Charger et nettoyer les données

Les valeurs manquantes sont représentées par `?` dans le fichier du cours. Elles doivent d’abord être converties en véritables valeurs manquantes. La variable `education` est supprimée parce qu’elle est redondante avec `educational.num` dans cet exemple.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import xgboost as xgb

adult = pd.read_csv("adult.csv")
adult = adult.replace("?", np.nan)
adult = adult.drop(columns=["education"])
```

Le cours regroupe ensuite les modalités rares de `native.country`. Ce regroupement est propre à ce jeu de données : il ne doit pas être repris automatiquement ailleurs.

```python
pays_vers_region = {
    "Cambodia": "Asia",
    "China": "Asia",
    "Hong": "Asia",
    "India": "Asia",
    "Iran": "Asia",
    "Japan": "Asia",
    "Laos": "Asia",
    "Philippines": "Asia",
    "Taiwan": "Asia",
    "Thailand": "Asia",
    "Vietnam": "Asia",
    "Columbia": "Center & South America",
    "Cuba": "Center & South America",
    "Dominican-Republic": "Center & South America",
    "Ecuador": "Center & South America",
    "El-Salvador": "Center & South America",
    "Guatemala": "Center & South America",
    "Haiti": "Center & South America",
    "Honduras": "Center & South America",
    "Jamaica": "Center & South America",
    "Mexico": "Center & South America",
    "Nicaragua": "Center & South America",
    "Peru": "Center & South America",
    "Puerto-Rico": "Center & South America",
    "Trinadad&Tobago": "Center & South America",
    "South": "Center & South America",
    "England": "Europe",
    "France": "Europe",
    "Germany": "Europe",
    "Greece": "Europe",
    "Holand-Netherlands": "Europe",
    "Hungary": "Europe",
    "Ireland": "Europe",
    "Italy": "Europe",
    "Poland": "Europe",
    "Portugal": "Europe",
    "Scotland": "Europe",
    "Yugoslavia": "Europe",
    "United-States": "Canada&USA",
    "Canada": "Canada&USA",
}

adult["native.country"] = adult["native.country"].replace(pays_vers_region)
```

---

## 2. Définir X et y

```python
X_brut = adult.drop(columns=["income"])

y = (
    adult["income"]
    .astype(str)
    .str.strip()
    .map({"<=50K": 0, ">50K": 1})
)

if y.isna().any():
    raise ValueError("La cible contient un libellé inattendu.")
```

```text
X_brut
→ variables explicatives quantitatives et catégorielles

y
→ 0 : revenu inférieur ou égal à 50K
→ 1 : revenu supérieur à 50K
```

---

## 3. Séparer train, validation et test

```python
from sklearn.model_selection import train_test_split

X_temp, X_valid_brut, y_temp, y_valid = train_test_split(
    X_brut,
    y,
    test_size=0.10,
    random_state=42,
    stratify=y,
)

X_train_brut, X_test_brut, y_train, y_test = train_test_split(
    X_temp,
    y_temp,
    test_size=0.20,
    random_state=42,
    stratify=y_temp,
)
```

Rôle des trois ensembles :

```text
TRAIN
→ apprend les arbres

VALIDATION
→ surveille l’apprentissage et déclenche l’arrêt anticipé

TEST
→ mesure une seule fois la performance finale
```

Le jeu de test ne doit pas figurer dans `evals` pour choisir le nombre d’itérations.

---

## 4. Encoder les variables catégorielles

L’interface native utilisée dans le cours attend des données numériques. Les colonnes indicatrices sont donc créées séparément, puis validation et test sont alignés sur les colonnes obtenues avec le train.

```python
X_train = pd.get_dummies(X_train_brut, dummy_na=True, dtype="int8")

X_valid = pd.get_dummies(X_valid_brut, dummy_na=True, dtype="int8")
X_valid = X_valid.reindex(columns=X_train.columns, fill_value=0)

X_test = pd.get_dummies(X_test_brut, dummy_na=True, dtype="int8")
X_test = X_test.reindex(columns=X_train.columns, fill_value=0)
```

Cette étape évite d’utiliser les catégories du test pour construire la représentation d’entraînement.

---

## 5. Construire les `DMatrix`

`xgb.train()` utilise les objets `DMatrix` de l’interface native XGBoost.

```python
dtrain = xgb.DMatrix(X_train, label=y_train, missing=np.nan)
dvalid = xgb.DMatrix(X_valid, label=y_valid, missing=np.nan)
dtest = xgb.DMatrix(X_test, label=y_test, missing=np.nan)
```

XGBoost peut gérer les valeurs manquantes numériques si elles sont correctement identifiées, ici par `np.nan`.

---

## 6. Définir les paramètres et entraîner

```python
params = {
    "booster": "gbtree",
    "objective": "binary:logistic",
    "eval_metric": "logloss",
    "eta": 0.05,
    "max_depth": 6,
    "subsample": 0.8,
    "colsample_bytree": 0.8,
    "seed": 42,
}

historique = {}

modele = xgb.train(
    params=params,
    dtrain=dtrain,
    num_boost_round=1000,
    evals=[(dtrain, "train"), (dvalid, "validation")],
    early_stopping_rounds=30,
    evals_result=historique,
    verbose_eval=50,
)
```

`early_stopping_rounds=30` arrête l’entraînement lorsque la métrique de validation ne s’améliore plus pendant 30 itérations. XGBoost utilise le **dernier ensemble** de `evals` pour cet arrêt : la validation doit donc être placée en dernier.

### Suivre les courbes d’apprentissage

```python
courbes = pd.DataFrame({
    "train": historique["train"]["logloss"],
    "validation": historique["validation"]["logloss"],
})

courbes.plot()
plt.xlabel("Itération de boosting")
plt.ylabel("Log loss")
plt.show()
```

Un écart croissant entre train et validation signale un risque de surapprentissage.

---

# Évaluer le modèle

## 7. Prédire sur le jeu de test

Avec l’interface native, le modèle retourné contient les itérations allant jusqu’à l’arrêt. Pour prédire avec la meilleure itération repérée sur la validation :

```python
if hasattr(modele, "best_iteration"):
    probabilites = modele.predict(
        dtest,
        iteration_range=(0, modele.best_iteration + 1),
    )
else:
    probabilites = modele.predict(dtest)

y_pred = (probabilites >= 0.5).astype(int)
```

`binary:logistic` renvoie la probabilité de la classe `1`. Le seuil `0.5` transforme ici cette probabilité en classe prédite.

---

## 8. Mesurer les performances

```python
from sklearn.metrics import (
    classification_report,
    confusion_matrix,
    roc_auc_score,
)

cm = confusion_matrix(y_test, y_pred, labels=[0, 1])
print(pd.DataFrame(cm, index=[0, 1], columns=[0, 1]))

print(classification_report(y_test, y_pred, zero_division=0))
print("ROC-AUC :", roc_auc_score(y_test, probabilites))
```

- La matrice de confusion décrit les types d’erreurs.
- Le rapport donne notamment précision, rappel et F1-score.
- La ROC-AUC évalue le classement produit par les probabilités, indépendamment du seul seuil `0.5`.

Une bonne accuracy ne suffit pas si les deux classes sont déséquilibrées ou si le coût des faux positifs et des faux négatifs diffère.

---

## 9. Examiner l’importance des variables

```python
xgb.plot_importance(
    modele,
    max_num_features=15,
    importance_type="gain",
)
plt.show()
```

![[Pasted image 20261002164604.png]]

Principaux modes de calcul :

- `weight` : nombre de divisions utilisant la variable ;
- `gain` : amélioration moyenne de la fonction de perte apportée par ces divisions ;
- `cover` : couverture moyenne des observations concernées ;
- `total_gain` et `total_cover` : versions cumulées.

Une importance élevée décrit l’utilisation d’une variable par le modèle. Elle ne démontre pas un lien causal et peut être influencée par la cardinalité ou les variables corrélées.

---

## Paramètres utiles

- `booster="gbtree"` : utilise des arbres comme apprenants faibles.
- `objective="binary:logistic"` : classification binaire avec probabilités en sortie.
- `eval_metric="logloss"` : métrique suivie pendant l’apprentissage.
- `eta` ou `learning_rate` : contribution de chaque nouvel arbre.
- `num_boost_round` : nombre maximal d’itérations ; c’est un argument de `xgb.train()`, pas une entrée de `params`.
- `max_depth` : profondeur maximale des arbres ; une valeur élevée augmente la complexité.
- `min_child_weight` : quantité minimale d’information requise dans un nœud enfant.
- `gamma` ou `min_split_loss` : réduction minimale de perte exigée pour créer une nouvelle division.
- `subsample` : fraction des observations utilisée à chaque itération.
- `colsample_bytree` : fraction des variables proposée à chaque arbre.
- `reg_alpha` : régularisation L1.
- `reg_lambda` : régularisation L2.
- `nthread` : nombre de threads de calcul pour l’interface native.
- `early_stopping_rounds` : patience avant l’arrêt lorsque la validation ne s’améliore plus.

Pour une régression à perte quadratique, l’objectif actuel est `reg:squarederror`. L’ancien nom `reg:linear` rencontré dans certains supports est obsolète.

---

# Pattern minimal

Les matrices ci-dessous sont déjà numériques et possèdent exactement les mêmes colonnes.

```python
import xgboost as xgb

dtrain = xgb.DMatrix(X_train_modele, label=y_train)
dvalid = xgb.DMatrix(X_valid_modele, label=y_valid)
dtest = xgb.DMatrix(X_test_modele)

params = {
    "objective": "binary:logistic",
    "eval_metric": "logloss",
    "eta": 0.05,
    "seed": 42,
}

modele = xgb.train(
    params=params,
    dtrain=dtrain,
    num_boost_round=1000,
    evals=[(dtrain, "train"), (dvalid, "validation")],
    early_stopping_rounds=30,
)

probabilites = modele.predict(
    dtest,
    iteration_range=(0, modele.best_iteration + 1),
)
y_pred = (probabilites >= 0.5).astype(int)
```

---

## Points de vigilance

- XGBoost appartient au package `xgboost`, même si son interface peut s’articuler avec les outils de `scikit-learn`.
- La validation sert à l’arrêt anticipé ; le test reste réservé à l’évaluation finale.
- Les variables catégorielles doivent être transformées de manière cohérente entre train, validation et test dans le déroulé présenté.
- Un `learning_rate` faible exige généralement davantage d’itérations ; il ne constitue pas une garantie contre le surapprentissage.
- Avec plusieurs jeux dans `evals`, le dernier pilote l’arrêt anticipé.
- Avec plusieurs métriques dans `eval_metric`, la dernière pilote l’arrêt anticipé.
- L’interface native ne sélectionne pas automatiquement la meilleure itération lors de `predict()` : utiliser `iteration_range` comme dans le déroulé.
- Les valeurs manquantes doivent être réellement codées comme telles ; la chaîne `"?"` n’est pas une valeur manquante pour le modèle.
- Une importance de variable n’est ni un effet causal ni une preuve que la variable est fiable hors échantillon.

---

## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de classification.md`.

- [Introduction au package Python XGBoost](https://xgboost.readthedocs.io/en/stable/python/python_intro.html)
- [API Python : `DMatrix`, `train` et `plot_importance`](https://xgboost.readthedocs.io/en/stable/python/python_api.html)
- [Paramètres XGBoost](https://xgboost.readthedocs.io/en/stable/parameter.html)
- [Prédire après un arrêt anticipé](https://xgboost.readthedocs.io/en/stable/prediction.html)
