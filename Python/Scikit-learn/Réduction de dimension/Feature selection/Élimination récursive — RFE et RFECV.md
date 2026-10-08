# Élimination récursive — RFE et RFECV

## Quand l’utiliser ?

Sélectionner des variables en s’appuyant sur un modèle ; fixer leur nombre ou le choisir par validation croisée.

## Principe

RFE réentraîne le modèle et élimine progressivement les variables les moins importantes. RFECV compare des tailles de sous-ensembles par validation croisée.

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

## 3. Éliminer progressivement les variables

Le modèle de base expose coef_. RFE retire les amplitudes les plus faibles et réentraîne. Le nombre 13 reprend l’exercice ; il est borné ici par le nombre disponible.

```python
from sklearn.feature_selection import RFE, RFECV
from sklearn.model_selection import KFold

selecteur = RFE(estimator=LinearRegression(), n_features_to_select=min(13, len(colonnes)), step=1)
selecteur.fit(X_train_modele, y_train)
X_train_reduit = selecteur.transform(X_train_modele)
X_test_reduit = selecteur.transform(X_test_modele)
colonnes_retenues = colonnes[selecteur.get_support()]
print(list(colonnes_retenues))
print(pd.Series(selecteur.ranking_, index=colonnes))
```

## Variante RFECV : laisser choisir le nombre de variables

```python
plis = KFold(n_splits=5, shuffle=True, random_state=2)
rfecv = RFECV(estimator=LinearRegression(), step=1, cv=plis, scoring="r2")
rfecv.fit(X_train_modele, y_train)
print("Nombre retenu :", rfecv.n_features_)
print(rfecv.cv_results_["mean_test_score"])
```

RFECV choisit le nombre de variables. Pour poursuivre avec son choix plutôt que RFE, transformer les deux jeux avec **rfecv**. Ici, comme dans le cours, le scaler est appris avant la CV interne : ses scores ont cette limite. Une CV rigoureuse doit réapprendre aussi le prétraitement dans chaque pli.

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

- `estimator` : modèle fournissant coef_ ou feature_importances_.
- `n_features_to_select` : nombre de colonnes à conserver ; 3 est à adapter.
- `step=1` : élimine une variable à chaque étape.
- `cv` : découpage de validation ; `scoring="r2"` correspond ici à une régression.
- `ranking_` : rang 1 pour les variables retenues, rang plus élevé pour celles éliminées plus tôt.

## Interprétation

Le résultat dépend du modèle et des données. Pour utiliser RFECV comme réduction, appeler aussi `rfecv.transform` sur les deux jeux.

# Pattern minimal

Avec les matrices standardisées :

```python
selecteur = RFE(LinearRegression(), n_features_to_select=3)
X_train_reduit = selecteur.fit_transform(X_train_modele, y_train)
X_test_reduit = selecteur.transform(X_test_modele)
```



## Points de vigilance

- Avec LinearRegression, les amplitudes des coefficients dépendent des unités : prévoir une standardisation adaptée.
- Avec une validation croisée, le prétraitement doit être réappris dans chaque pli. Standardiser une fois tout le train avant la CV laisse les plis de validation influencer le prétraitement ; le test final reste séparé, mais le score de CV peut être optimiste. Les pipelines seront traités ultérieurement.
- RFE peut être coûteux ; step plus grand accélère mais rend la recherche moins fine.
- Comparer la sélection à une référence sans sélection sur validation ; le test ne sert pas à multiplier les essais. La sélection doit elle aussi être réapprise dans chaque pli.

## Références

Source de formation : `01_RESSOURCES/Liora/Réduction de dimension.md`.

- [feature_selection.RFE](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.RFE.html)
- [feature_selection.RFECV](https://scikit-learn.org/stable/modules/generated/sklearn.feature_selection.RFECV.html)

