# Elastic Net

## Quand l’utiliser ?

Prédire une cible quantitative en régularisant un modèle linéaire. Comparer avec la régression linéaire non pénalisée.

## Principe

Elastic Net mélange les pénalités L1 et L2 ; ElasticNetCV recherche la force et le mélange par validation interne.

> Exemple NBA : points marqués (pts).

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

---

# Pas à pas

## 1. Préparer les données et définir X / y

Le support crée les indicatrices de position avant découpage. Cette reproduction suppose un schéma de positions connu ; si les catégories doivent être découvertes, apprendre l’encodeur seulement sur le train. Vérifier aussi que les statistiques explicatives seraient disponibles au moment réel de la prédiction.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("nba_2013.csv").dropna().copy()

X = df.drop(columns=["pts", "pos_SG"])
y = df["pts"]
```

## 2. Séparer train et test

`X` est une matrice 2D, `y` un vecteur 1D. Les quatre sorties restent alignées. Le train sert à apprendre ; le test reste à l’écart du choix des réglages. En classification, on peut ajouter `stratify=y` si les effectifs le permettent ; le découpage ci-dessous reprend celui du cours.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=101
)
```


## 3. Préparer la représentation

Les distances ou la pénalisation dépendent des unités. `fit_transform` apprend les moyennes et écarts-types **sur le train**, puis le transforme. `transform` réutilise ces valeurs sur le test ; le test n’a pas à être exactement centré-réduit.

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)
```

## 4. Instancier, entraîner et prédire

Le modèle apprend une cible quantitative. predict retourne des nombres, jamais des probabilités de classes.

```python
from sklearn.linear_model import ElasticNetCV

modele = ElasticNetCV(l1_ratio=[0.1, 0.5, 0.9, 0.99], alphas=[0.001, 0.01, 0.1, 1.0], cv=8, max_iter=10000)
modele.fit(X_train_scaled, y_train)
y_pred = modele.predict(X_test_scaled)
```

## Lire le réglage retenu

```python
alpha_retenu = modele.alpha_
print("Alpha retenu :", alpha_retenu)
print("Part L1 retenue :", modele.l1_ratio_)
```

La CV interne réutilise ici les données standardisées comme dans le support : cela protège le test final, mais la standardisation globale du train peut influencer les scores des plis. Pour une sélection rigoureuse, le prétraitement doit être réappris dans chaque pli ; les pipelines restent un approfondissement ultérieur.

## Lire les coefficients

```python
coefficients = pd.Series(modele.coef_, index=X.columns)
print(coefficients)
print("Constante :", modele.intercept_)
```

L’ordre des coefficients suit les colonnes de X. Après standardisation, une unité représente un écart-type du train. Un signe positif ou négatif décrit l’association conditionnelle dans le modèle, pas une causalité.

# Évaluer le modèle

## R², MSE et RMSE

R² peut être négatif. MSE est une erreur au carré ; RMSE revient dans l’unité de la cible. Comparer les valeurs sur le même jeu de test.

```python
from sklearn.metrics import mean_squared_error

mse = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)
r2 = modele.score(X_test_modele, y_test)
print({"R2": r2, "MSE": mse, "RMSE": rmse})
```

## Examiner les résidus

Convention : réel − prédit. Un motif courbe, un éventail ou quelques erreurs extrêmes orientent les vérifications.

```python
residus = y_test - y_pred
plt.scatter(y_pred, residus)
plt.axhline(0, color="black", linestyle="--")
plt.xlabel("Valeurs prédites")
plt.ylabel("Résidus : réel - prédit")
plt.show()
```

## Comparer apprentissage et test

```python
prediction_train = modele.predict(X_train_modele)
print("RMSE train :", np.sqrt(mean_squared_error(y_train, prediction_train)))
print("RMSE test :", rmse)
```

Un grand écart peut signaler du surapprentissage, mais un seul découpage ne suffit pas à conclure.

## Paramètres utiles

- `l1_ratio` : part L1 ; 1 = Lasso, valeurs intermédiaires = mélange. La limite 0 correspond à L2 mais n’est pas un raccourci recommandé pour régler Ridge avec ElasticNetCV.
- `alphas` : forces de pénalisation à tester ; `cv=8` : huit plis.
- `max_iter` : plafond d’itérations.

## Interprétation

`alpha_` et `l1_ratio_` sont les réglages retenus. `coef_`, `intercept_`, `alphas_` et `mse_path_` permettent d’examiner le résultat.

# Pattern minimal

Les matrices ci-dessous ont déjà été préparées avec le **même objet appris sur le train** ; elles ne désignent pas les données brutes. 

```python
from sklearn.linear_model import ElasticNetCV

modele = ElasticNetCV(l1_ratio=[0.1, 0.5, 0.9, 0.99], alphas=[0.001, 0.01, 0.1, 1.0], cv=8, max_iter=10000)
modele.fit(X_train_modele, y_train)
y_pred = modele.predict(X_test_modele)
```



## Points de vigilance

- Standardiser les variables quantitatives avant d’interpréter ou comparer les pénalités.
- Avec une validation croisée, le prétraitement doit être réappris dans chaque pli. Standardiser une fois tout le train avant la CV laisse les plis de validation influencer le prétraitement ; le test final reste séparé, mais le score de CV peut être optimiste. Les pipelines seront traités ultérieurement.
- Un alpha optimal dans la grille n’est pas un optimum universel ; examiner les limites de la grille.
- Pour Lasso/Elastic Net, la sélection entre variables corrélées peut être instable ; un coefficient nul ne prouve pas une absence de lien.


## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de regression.md`.

- [linear_model.ElasticNetCV](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.ElasticNetCV.html)

