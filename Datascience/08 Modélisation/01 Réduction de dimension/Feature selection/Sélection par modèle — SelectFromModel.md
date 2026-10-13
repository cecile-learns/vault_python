# Sélection par modèle — SelectFromModel

## Quand l’utiliser ?

Retenir les variables dont le modèle estime l’importance suffisante. Exemple du cours : coefficients d’un Lasso.

## Principe

SelectFromModel filtre à partir des amplitudes de coef_ ou des importances du modèle.

> Cours : real_estate_boston.csv, cible medv. Les variables retenues gardent leur signification initiale.

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

---

# Pas à pas

## 1. Préparer les données et réserver le test

Supprimer ID, qui identifie une observation sans être une caractéristique explicative. Le séparateur de ce fichier est le point-virgule.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression

df = pd.read_csv("real_estate_boston.csv", sep=";").dropna()
X = df.drop(columns=["ID", "medv"])
y = df["medv"]
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
colonnes = X_train.columns
```

## 2. Standardiser pour comparer les coefficients

Les distances ou la pénalisation dépendent des unités. `fit_transform` apprend les moyennes et écarts-types **sur le train**, puis le transforme. `transform` réutilise ces valeurs sur le test ; le test n’a pas à être exactement centré-réduit.

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_train_modele = scaler.fit_transform(X_train)
X_test_modele = scaler.transform(X_test)
```

## 3. Sélectionner à partir du Lasso

La sélection repose sur les amplitudes des coefficients, y compris négatifs. Une pénalité forte peut annuler beaucoup de coefficients.

```python
from sklearn.feature_selection import SelectFromModel
from sklearn.linear_model import Lasso

selecteur = SelectFromModel(Lasso(alpha=1.0, max_iter=10000), threshold=1e-10)
selecteur.fit(X_train_modele, y_train)
colonnes_retenues = colonnes[selecteur.get_support()]
assert len(colonnes_retenues) > 0, "Aucune variable retenue : revoir alpha/seuil sur validation."
X_train_reduit = selecteur.transform(X_train_modele)
X_test_reduit = selecteur.transform(X_test_modele)
print(list(colonnes_retenues))
```

## Réutiliser un modèle déjà ajusté

Après une recherche d’alpha sur le train, le cours utilise `SelectFromModel(estimator=recherche.best_estimator_, prefit=True, threshold=1e-10)`. Avec prefit=True, appeler transform ; ne pas refaire fit sur le test.

## 4. Entraîner sur les variables conservées

Ce prolongement opérationnel montre comment juger la sélection avec un modèle final. Les colonnes du test sont exactement celles sélectionnées sur le train.

```python
modele = LinearRegression()
modele.fit(X_train_reduit, y_train)
y_pred = modele.predict(X_test_reduit)
```

# Évaluer le modèle

## R², MSE et RMSE

R² peut être négatif. MSE est une erreur au carré ; RMSE revient dans l’unité de la cible. Comparer les valeurs sur le même jeu de test.

```python
from sklearn.metrics import mean_squared_error

mse = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)
r2 = modele.score(X_test_reduit, y_test)
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

## Paramètres utiles

- `estimator` : modèle utilisé pour mesurer l’importance.
- `alpha` : pénalisation L1 du Lasso ; `max_iter` : plafond d’optimisation.
- `threshold=1e-10` : retient les coefficients d’amplitude suffisante, proche d’une sélection des non-nuls.
- `prefit=True` : uniquement si l’estimateur fourni est déjà entraîné ; utiliser alors transform sans réentraîner le sélecteur.

## Interprétation

Un grand coefficient négatif peut être retenu : c’est son amplitude qui compte. Un coefficient nul est lié au modèle et à la pénalité, pas une preuve d’inutilité universelle.

# Pattern minimal

Après préparation :

```python
selecteur = SelectFromModel(Lasso(alpha=1.0), threshold=1e-10)
X_train_reduit = selecteur.fit_transform(X_train_modele, y_train)
X_test_reduit = selecteur.transform(X_test_modele)
```



## Points de vigilance

- Un seuil trop fort peut ne laisser aucune variable.
- Standardisation importante pour comparer les coefficients.
- La sélection doit être réapprise dans chaque pli lors d’une évaluation par CV.
- Comparer la sélection à une référence sans sélection sur validation ; le test ne sert pas à multiplier les essais. La sélection doit elle aussi être réapprise dans chaque pli.

## Références

Source de formation : `01_RESSOURCES/Liora/Réduction de dimension.md`.

- [feature_selection.SelectFromModel](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.SelectFromModel.html)
- [linear_model.Lasso](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.Lasso.html)

