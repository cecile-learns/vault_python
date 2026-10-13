# Sélectionner les variables — Filter, Wrapper et Embedded

## Quand l'utiliser ?

Pour réduire le nombre de variables explicatives `X` lorsqu'elles sont constantes, redondantes, peu informatives ou lorsque leur nombre pénalise le modèle.

Trois familles :

|Famille|Principe|Méthodes|
|---|---|---|
|**Filter**|Sélection selon les propriétés statistiques des variables|`VarianceThreshold`, `SelectKBest`, `SelectPercentile`|
|**Wrapper**|Sélection en utilisant un modèle entraîné|`RFE`, `RFECV`|
|**Embedded**|Sélection selon l'importance apprise par un modèle|`SelectFromModel`|

**Principe :** sélectionner moins de variables n'est pas un objectif en soi. Comparer les performances avec et sans sélection.

Pour choisir une méthode selon la situation : [[Préprocessing des données/2. Feature selection/Choisir une méthode de Feature Selection]].

# Pas à pas

## 1. Préparer les données et réserver le test

Exemple : prédire `medv`, valeur médiane des logements, à partir des variables du dataset immobilier de Boston.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from sklearn.model_selection import (
    train_test_split, GridSearchCV, KFold
)
from sklearn.feature_selection import (
    VarianceThreshold, SelectKBest, SelectPercentile,
    f_regression, mutual_info_regression,
    RFE, RFECV, SelectFromModel
)
from sklearn.linear_model import LinearRegression, Lasso
from sklearn.pipeline import Pipeline

df = pd.read_csv("real_estate_boston.csv", sep=";")

X = df.drop(columns=["ID", "medv"])
y = df["medv"]

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
```

Les méthodes présentées ensuite constituent **des alternatives**, et non des opérations à enchaîner systématiquement.

Les exemples supposent des variables numériques sans valeurs manquantes. Si nécessaire, intégrer une imputation dans le pipeline.

## 2. FILTER — Retirer les variables constantes ou quasi constantes

### `VarianceThreshold`

**Principe :** sélectionner les variables dont la variance dépasse un seuil. La cible `y` n'est pas utilisée.

```python
sel = VarianceThreshold(threshold=0)

sel.fit(X_train)

# Colonnes conservées
colonnes = X_train.columns[sel.get_support()]
print(colonnes.tolist())

# Transformations
X_train_filtre = X_train[colonnes]
X_test_filtre = X_test[colonnes]
```

`threshold=0` élimine uniquement les variables constantes.

Pour éliminer les variables à faible variance, définir un seuil supérieur à zéro, adapté à l'échelle des variables.

### Visualiser les variables sélectionnées

```python
mask = sel.get_support()

plt.matshow(mask.reshape(1, -1), cmap="gray_r")
plt.xticks(range(len(X_train.columns)),
           X_train.columns, rotation=90)
plt.yticks([])
plt.xlabel("Variables")
plt.show()
```

- `True` : variable conservée.
    
- `False` : variable éliminée.
    

**Vigilance :** une faible variance ne signifie pas nécessairement une faible utilité prédictive. Les variances dépendent aussi des unités de mesure.

## 3. FILTER — Repérer les variables fortement corrélées entre elles

### Matrice de corrélation

**Principe :** identifier les variables quantitatives qui apportent potentiellement une information redondante.

```python
corr = X_train.corr()

plt.figure(figsize=(10, 8))
plt.imshow(corr, cmap="coolwarm", vmin=-1, vmax=1)
plt.colorbar()

plt.xticks(range(len(corr.columns)),
           corr.columns, rotation=90)
plt.yticks(range(len(corr.index)),
           corr.index)

plt.show()
```

**Interprétation :**

- Corrélation proche de `1` : forte relation linéaire positive.
    
- Corrélation proche de `-1` : forte relation linéaire négative.
    
- Corrélation proche de `0` : absence de relation linéaire marquée.
    

Un seuil exploratoire comme `|r| ≥ 0.8` permet de repérer les paires fortement corrélées.

**Ce que je fais :**

1. Repérer les groupes de variables corrélées.
    
2. Examiner leur pertinence métier et leur relation avec la cible.
    
3. Envisager de supprimer certaines variables redondantes.
    
4. Comparer les performances du modèle avant/après.
    

**Vigilance :** ne pas supprimer automatiquement une variable parce qu'elle est corrélée à une autre. La multicolinéarité pose notamment des problèmes de stabilité et d'interprétation des coefficients des modèles linéaires.

## 4. FILTER — Sélectionner selon la relation avec la cible

### `SelectKBest` / `SelectPercentile`

**Principe :** calculer un score individuel entre chaque variable `X` et la cible `y`, puis conserver les mieux classées.

- `SelectKBest` : conserve `k` variables.
    
- `SelectPercentile` : conserve un pourcentage de variables.
    

### Choisir la fonction de score

|Cible `y`|Variables `X`|`score_func`|À choisir quand…|
|---|---|---|---|
|Quantitative|Numériques|`f_regression`|Relation linéaire avec la cible|
|Quantitative|Numériques|`mutual_info_regression`|Dépendance éventuellement non linéaire|
|Catégorielle|Numériques|`f_classif`|Comparaison des moyennes entre classes (ANOVA)|
|Catégorielle|Numériques|`mutual_info_classif`|Dépendance générale avec les classes|
|Catégorielle|Non négatives, adaptées au χ²|`chi2`|Comptages, fréquences ou caractéristiques non négatives appropriées|

`f_classif` correspond au score F de l'ANOVA : inutile d'ajouter une ligne `anova`.

### Exemple — SelectKBest

```python
sel = SelectKBest(score_func=f_regression, k=5)

sel.fit(X_train, y_train)

# Variables retenues
colonnes = sel.get_feature_names_out()
print(colonnes)

# Scores de toutes les variables
scores = pd.Series(
    sel.scores_, index=X_train.columns
).sort_values(ascending=False)

display(scores)

# Transformation
X_train_sel = sel.transform(X_train)
X_test_sel = sel.transform(X_test)
```

Ici, on conserve les cinq variables ayant les meilleurs scores F.

### Alternative — Information mutuelle

```python
sel = SelectKBest(
    score_func=mutual_info_regression,
    k=5
)

sel.fit(X_train, y_train)
print(sel.get_feature_names_out())
```

### Alternative — SelectPercentile

```python
sel = SelectPercentile(
    score_func=f_regression,
    percentile=50
)

sel.fit(X_train, y_train)
print(sel.get_feature_names_out())
```

**Vigilance :** ces méthodes évaluent les variables individuellement. Elles ne tiennent pas directement compte de leur contribution combinée dans le modèle.

### Autres méthodes de filtrage rencontrées

|Méthode|Variables|Cible utilisée ?|Utilité|
|---|---|---|---|
|Score de Fisher|Numériques|Oui, catégorielle|Classer selon la séparation entre classes|
|Différence absolue moyenne (MAD)|Numériques|Non|Mesurer la dispersion autour de la moyenne|
|Rapport de dispersion AM/GM|Numériques strictement positives|Non|Comparer des mesures de dispersion|
|Kruskal-Wallis|Numériques / ordinales|Oui, catégorielle|Tester les différences de distributions entre groupes|
|V de Cramer|Catégorielles|Oui, catégorielle|Mesurer la force d'association entre variables catégorielles|
```Python
#Score de Fisher
from skfeature.function.similarity_based import fisher_score
ranks = fisher_score.fisher_score(np.array(X), np.array(y))
ranks

#Différence absolue moyenne
mad = np.sum(np.abs(X - np.mean(X, axis =0)), axis = 0)/X.shape[0]

#Test de Kruskal Wallis
from scipy.stats import kruskal
feature_scores = {}
feat_select = []
for feature in X.columns:
    groups = [X[feature][y == level] for level in y.unique()]
    kruskal_statistic, p_value = kruskal(*groups)
    feature_scores[feature] = p_value
    
    if p_value <= 0.5 :
        feat_select.append(feature)

sorted_features = sorted(feature_scores.items(), key=lambda x: x[1])
sorted_features

#Vcramer
from scipy.stats import chi2_contingency

# Calculer la matrice de contingence pour chaque paire de fonctionnalités
contingency_matrices = {}
for feature in X.columns:
    contingency_matrix = pd.crosstab(X[feature], y)
    contingency_matrices[feature] = contingency_matrix

# Calculer le V de Cramer pour chaque paire de fonctionnalités
feature_scores = {}
for feature, contingency_matrix in contingency_matrices.items():
    # Calculer le chi2 et le p-value
    chi2, _, _, _ = chi2_contingency(contingency_matrix)
    n = contingency_matrix.sum().sum()
    # Calculer le V de Cramer
    cramer_v = np.sqrt(chi2 / (n * (min(contingency_matrix.shape) - 1)))
    feature_scores[feature] = cramer_v

# Trier les fonctionnalités en fonction de leur score de V de Cramer
sorted_features = sorted(feature_scores.items(), key=lambda x: x[1], reverse=True)
sorted_features

```
Ces méthodes appartiennent à la famille **Filter**, mais ne sont pas toutes directement disponibles comme `score_func` dans scikit-learn.

- Le score de Fisher est proche dans son principe du score F d'ANOVA.
    
- MAD et rapport AM/GM ne mesurent pas directement la relation avec la cible.
    
- Kruskal-Wallis peut être utilisé comme test de filtrage non paramétrique, sous réserve de vérifier les conditions d'application.
    
- Le V de Cramer est une mesure d'association, pas une fonction de sélection sklearn prête à l'emploi.
    

**Attention :** une petite p-value n'est pas une mesure directe du pouvoir prédictif d'une variable.

## 5. WRAPPER — Sélection selon un modèle

### RFE — Recursive Feature Elimination

**Principe :** entraîner un modèle, éliminer progressivement les variables les moins importantes, puis réentraîner jusqu'à atteindre le nombre souhaité de variables.

```python
lm = LinearRegression()

rfe = RFE(
    estimator=lm,
    n_features_to_select=5,
    step=1
)

rfe.fit(X_train, y_train)

print("Variables retenues :",
      rfe.get_feature_names_out())

print("Classement :",
      rfe.ranking_)
```

- `ranking_ = 1` : variable sélectionnée.
    
- `ranking_ > 1` : variable éliminée, avec son rang d'élimination.
## 5. WRAPPER — Sélection selon les performances d'un modèle

**Principe :** les méthodes Wrapper entraînent un modèle sur différents sous-ensembles de variables pour identifier celles à conserver.

Contrairement aux méthodes Filter, elles évaluent les variables en fonction d'un estimateur.

|Méthode|Fonctionnement|Particularité|
|---|---|---|
|RFE|Élimine progressivement les variables les moins importantes|Nombre de variables fixé|
|RFECV|RFE associé à une validation croisée|Choisit le nombre de variables|
|Forward Selection|Part de zéro et ajoute progressivement des variables|Sélection ascendante|
|Backward Selection|Part de toutes les variables et les retire progressivement|Sélection descendante|
|Exhaustive Selection|Évalue toutes les combinaisons autorisées|Coût de calcul élevé|

### 5.1 RFE — Recursive Feature Elimination

**Principe :** entraîner un modèle, supprimer les variables les moins importantes selon ses coefficients ou importances, puis recommencer jusqu'au nombre souhaité.

```python
from sklearn.feature_selection import RFE
from sklearn.linear_model import LinearRegression

modele = LinearRegression()

rfe = RFE(
    estimator=modele,
    n_features_to_select=5,
    step=1
)

rfe.fit(X_train, y_train)

print("Variables retenues :", rfe.get_feature_names_out())
print("Classement :", rfe.ranking_)
```

- `ranking_ = 1` : variable retenue.
    
- `ranking_ > 1` : variable éliminée.
    
- `step=1` : une variable supprimée à chaque itération.
    

**Visualisation :**

```python
plt.matshow(rfe.ranking_.reshape(1, -1), cmap="gray_r")
plt.xticks(range(X_train.shape[1]), X_train.columns, rotation=90)
plt.yticks([])
plt.colorbar(label="Rang")
plt.show()
```

### 5.2 RFECV — Recursive Feature Elimination with Cross-Validation

**Principe :** reprend RFE mais utilise la validation croisée pour déterminer le nombre de variables à conserver selon les performances du modèle.

```python
from sklearn.feature_selection import RFECV
from sklearn.model_selection import KFold

cv = KFold(n_splits=5, shuffle=True, random_state=42)

rfecv = RFECV(
    estimator=LinearRegression(),
    cv=cv,
    step=1,
    scoring="neg_mean_squared_error"
)

rfecv.fit(X_train, y_train)

print("Nombre de variables :", rfecv.n_features_)
print("Variables retenues :", rfecv.get_feature_names_out())
```

**Visualiser l'évolution des performances :**

```python
resultats = rfecv.cv_results_

plt.plot(
    resultats["n_features"],
    -resultats["mean_test_score"],
    marker="o"
)

plt.xlabel("Nombre de variables")
plt.ylabel("MSE moyenne en validation croisée")
plt.title("RFECV — Performance selon le nombre de variables")
plt.show()
```

La MSE est affichée en positif : **plus elle est faible, meilleure est la performance**.

### 5.3 Forward Sequential Selection

**Principe :** partir d'aucune variable, puis ajouter progressivement celle qui améliore le plus le score de validation croisée du modèle.

```python
from sklearn.feature_selection import SequentialFeatureSelector

modele = LinearRegression()

sfs = SequentialFeatureSelector(
    estimator=modele,
    n_features_to_select=3,
    direction="forward",
    scoring="neg_mean_squared_error",
    cv=5
)

sfs.fit(X_train, y_train)

print("Variables retenues :", sfs.get_feature_names_out())
```

Ici, on conserve trois variables, sélectionnées progressivement.

### 5.4 Backward Sequential Selection

**Principe :** partir de toutes les variables, puis retirer progressivement celle dont la suppression donne le meilleur score de validation croisée.

```python
sbs = SequentialFeatureSelector(
    estimator=LinearRegression(),
    n_features_to_select="auto",
    direction="backward",
    scoring="neg_mean_squared_error",
    cv=5
)

sbs.fit(X_train, y_train)

print("Variables retenues :", sbs.get_feature_names_out())
```

**Attention :** `n_features_to_select="auto"` conserve par défaut environ la moitié des variables lorsqu'aucune tolérance `tol` n'est définie. Cela ne recherche pas automatiquement le nombre optimal de variables.

### 5.5 Exhaustive Feature Selection (EFS)

**Principe :** tester toutes les combinaisons possibles de variables dans les limites définies par `min_features` et `max_features`.

```python
from mlxtend.feature_selection import ExhaustiveFeatureSelector as EFS

modele = LinearRegression()

efs = EFS(
    estimator=modele,
    min_features=1,
    max_features=4,
    scoring="neg_mean_squared_error",
    cv=5,
    print_progress=True
)

efs.fit(X_train, y_train)

print("Variables retenues :", efs.best_feature_names_)
print("Meilleur score :", efs.best_score_)
```

Dans cet exemple, toutes les combinaisons de 1 à 4 variables sont évaluées.

**Vigilance :** le nombre de combinaisons augmente très rapidement avec le nombre de variables. Cette méthode peut devenir très coûteuse en calcul.

`EFS` appartient à la bibliothèque `mlxtend`, distincte de scikit-learn.

### Paramètres et attributs utiles

|Paramètre / attribut|Fonction|
|---|---|
|`estimator`|Modèle utilisé pour sélectionner les variables|
|`n_features_to_select`|Nombre de variables à conserver|
|`direction`|`"forward"` ou `"backward"`|
|`step`|Nombre ou proportion de variables éliminées à chaque étape RFE|
|`cv`|Nombre de plis ou stratégie de validation croisée|
|`scoring`|Métrique utilisée pour comparer les sous-ensembles|
|`ranking_`|Classement des variables avec RFE|
|`n_features_`|Nombre de variables retenues par RFECV|
|`get_feature_names_out()`|Noms des variables sélectionnées|
|`best_feature_names_`|Meilleure combinaison retenue par EFS|

### Points de vigilance

- Utiliser uniquement `X_train` et `y_train` pour apprendre la sélection. Ne jamais utiliser le jeu de test pour choisir les variables.
    
- Adapter le modèle et le `scoring` à la cible : régression ou classification.
    
- RFE nécessite un modèle fournissant une importance des variables (`coef_` ou `feature_importances_`).
    
- Les méthodes séquentielles Forward et Backward évaluent les sous-ensembles à l'aide des performances du modèle, pas nécessairement de coefficients.
    
- Les méthodes Wrapper sont généralement plus coûteuses que les méthodes Filter.
    
- Lorsque la sélection fait partie d'une validation croisée externe ou d'une recherche d'hyperparamètres, l'intégrer dans un `Pipeline` afin qu'elle soit recalculée dans chaque pli.
    
- Comparer les performances avec une référence sans sélection avant de retenir la méthode.
    

**À retenir :** RFE retire selon l'importance des variables ; Forward et Backward ajoutent ou retirent selon les performances ; RFECV choisit un nombre de variables par validation croisée ; EFS explore exhaustivement les combinaisons autorisées.

### Visualisation

```python
plt.matshow(
    rfe.ranking_.reshape(1, -1),
    cmap="gray_r"
)

plt.xticks(
    range(X_train.shape[1]),
    X_train.columns,
    rotation=90
)

plt.yticks([])
plt.colorbar(label="Rang")
plt.show()
```

**Limite :** le nombre de variables à conserver doit être fixé.

### RFECV — RFE avec validation croisée

**Principe :** utilise RFE et la validation croisée pour choisir le nombre de variables retenues selon les performances.

```python
cv = KFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)

rfecv = RFECV(
    estimator=LinearRegression(),
    cv=cv,
    step=1,
    scoring="neg_mean_squared_error"
)

rfecv.fit(X_train, y_train)

print("Nombre retenu :", rfecv.n_features_)
print("Variables :", rfecv.get_feature_names_out())
```

### Visualiser la performance selon le nombre de variables

```python
resultats = rfecv.cv_results_

plt.plot(
    resultats["n_features"],
    -resultats["mean_test_score"],
    marker="o"
)

plt.xlabel("Nombre de variables")
plt.ylabel("MSE moyenne en validation croisée")
plt.title("RFECV — Performance selon le nombre de variables")
plt.show()
```

Avec `neg_mean_squared_error`, on inverse le signe pour afficher une MSE positive. Une MSE plus faible indique une meilleure performance.

**Vigilance :** RFECV est plus coûteux en calcul que les méthodes Filter. Le résultat dépend du modèle, de la métrique et de la validation croisée choisis.

## 6. EMBEDDED — Sélection à partir de l'importance du modèle

### `SelectFromModel`

**Principe :** conserver les variables dont l'importance apprise par un estimateur dépasse un seuil.

Estimateurs utilisables notamment :

- Lasso : coefficients `coef_` pouvant devenir nuls.
    
- Régression logistique pénalisée L1 : coefficients `coef_`.
    
- Random Forest : importance `feature_importances_`.
    

### Exemple — Lasso

```python
lasso = Lasso(alpha=1, max_iter=10000)

sel = SelectFromModel(
    estimator=lasso,
    threshold=1e-10
)

sel.fit(X_train, y_train)

print(sel.get_feature_names_out())
```

Ici, le seuil proche de zéro permet de sélectionner les variables dont les coefficients ne sont pas considérés comme nuls.

**Vigilance :** Lasso est sensible à l'échelle des variables : privilégier une standardisation intégrée au pipeline.

### Optimiser alpha avant sélection

```python
pipe = Pipeline([
    ("scaler", StandardScaler()),
    ("lasso", Lasso(max_iter=10000))
])

grid_search = GridSearchCV(
    estimator=pipe,
    param_grid={
        "lasso__alpha": [0.01, 0.1, 1, 10]
    },
    scoring="neg_mean_squared_error",
    cv=5
)

grid_search.fit(X_train, y_train)

print(grid_search.best_params_)

coefficients = grid_search.best_estimator_[
    "lasso"
].coef_

variables = X_train.columns[
    np.abs(coefficients) > 1e-10
]

print("Variables retenues :", variables.tolist())
```

Ajouter à la liste des imports :

```python
from sklearn.preprocessing import StandardScaler
```

Ce second exemple effectue une sélection à partir des coefficients du Lasso optimisé ; il ne nécessite pas de réentraîner un second `SelectFromModel`.

# Paramètres et attributs utiles

|Paramètre / attribut|Fonction|
|---|---|
|`threshold`|Seuil de variance ou d'importance selon le sélecteur|
|`score_func`|Fonction qui attribue un score aux variables|
|`k`|Nombre de variables conservées|
|`percentile`|Pourcentage de variables conservées|
|`step`|Nombre ou proportion de variables éliminées par itération RFE|
|`n_features_to_select`|Nombre cible de variables|
|`get_support()`|Masque booléen des variables sélectionnées|
|`get_feature_names_out()`|Noms des variables conservées|
|`scores_`|Scores individuels de sélection, si disponibles|
|`ranking_`|Classement des variables avec RFE|
|`n_features_`|Nombre de variables retenues, notamment par RFECV|

# Points de vigilance

1. **Train / test** : apprendre la sélection sur le train et appliquer exactement la même transformation au test.
    
2. **Validation croisée** : intégrer la sélection dans chaque pli, idéalement avec un `Pipeline`. Ne pas sélectionner une fois sur tout le train avant de lancer une CV sur ces données déjà sélectionnées.
    
3. **Variance** : un filtre par faible variance dépend de l'échelle des variables. La standardisation préalable peut masquer les différences de variance.
    
4. **Relation avec y** : choisir une fonction de score compatible avec les types de X et de y.
    
5. **Redondance** : une forte corrélation entre deux variables ne signifie pas qu'une suppression est automatiquement nécessaire.
    
6. **Comparaison** : évaluer un modèle avec et sans sélection, sur validation ou validation croisée.
    
7. **Test final** : ne l'utiliser qu'après avoir fixé la méthode, les variables et les hyperparamètres.
    

### Pattern recommandé avec validation croisée

```python
pipeline = Pipeline([
    ("selection", SelectKBest(
        score_func=f_regression,
        k=5
    )),
    ("modele", LinearRegression())
])

grid = GridSearchCV(
    estimator=pipeline,
    param_grid={"selection__k": [3, 5, 8]},
    scoring="neg_mean_squared_error",
    cv=5
)

grid.fit(X_train, y_train)

print(grid.best_params_)
print(grid.best_score_)
```

La sélection est alors recalculée dans chaque pli de validation croisée, sans fuite provenant du calcul des scores de sélection.

# Références

Support Liora : `01_RESSOURCES/Liora/Réduction de dimension.md`

- [VarianceThreshold](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.VarianceThreshold.html)
    
- [SelectKBest](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectKBest.html)
    
- [RFE](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.RFE.html)
    
- [RFECV](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.RFECV.html)
    
- [SelectFromModel](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectFromModel.html)
    
- [Pipeline](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html)