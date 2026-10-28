# Lasso

## Quand l’utiliser ?

Prédire une cible quantitative en régularisant un modèle linéaire. Comparer avec la régression linéaire non pénalisée.

## Principe

Une pénalité L1 peut annuler des coefficients ; LassoCV choisit sa force par validation interne.
Le modèle Lasso est similaire au modèle Ridge mais sa contrainte est sur la norme L1 et non sur la norme L2.

La régression Lasso peut ainsi s'écrire :

min𝛽(||𝑌−𝑋𝛽||2+𝛼||𝛽||1)
> 

---

# Pas à pas

## 1. Préparer les données et définir X / y

Le fichier du cours contient déjà les caractéristiques numériques. La cible price ne doit pas rester dans X. La suppression de lignes incomplètes est un choix illustratif, pas une règle générale.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("automobiles.csv")

X = df.drop(columns=["price"])
y = df["price"]
```

## 2. Séparer train et test

`X` est une matrice 2D, `y` un vecteur 1D. Les quatre sorties restent alignées. Le train sert à apprendre ; le test reste à l’écart du choix des réglages. En classification, on peut ajouter `stratify=y` si les effectifs le permettent ; le découpage ci-dessous reprend celui du cours.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=789
)

```

## 3. Préparer la représentation

Les distances ou la pénalisation dépendent des unités. `fit_transform` apprend les moyennes et écarts-types **sur le train**, puis le transforme. `transform` réutilise ces valeurs sur le test ; le test n’a pas à être exactement centré-réduit.

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

X_train_scaled = pd.DataFrame(
    scaler.fit_transform(X_train),
    columns=X_train.columns,
    index=X_train.index
)

X_test_scaled = pd.DataFrame(
    scaler.transform(X_test),
    columns=X_test.columns,
    index=X_test.index
)
```

## 4. Instancier, entraîner et prédire

Le modèle apprend une cible quantitative. predict retourne des nombres, jamais des probabilités de classes.

```python
from sklearn.linear_model import Lasso, LassoCV

# Essai à pénalité fixée, puis choix interne par CV.
essai = Lasso(alpha=1.0, max_iter=10000).fit(X_train_modele, y_train)


modele = LassoCV(alphas=[0.001, 0.01, 0.1, 1.0, 10.0], cv=10, max_iter=10000)


modele.fit(X_train, y_train)
y_pred = modele.predict(X_test)
```

## Lire le réglage retenu

```python

alpha = modele.alpha_


plt.figure(figsize = (10, 8))

plt.plot(alphas, model_lasso.mse_path_, ':')
plt.plot(alphas, model_lasso.mse_path_.mean(axis = 1), 'k', label = 'Moyenne', linewidth = 2)

plt.axvline(model_lasso.alpha_, linestyle = '--', color = 'k', label = 'alpha : estimation CV')

plt.xlabel('Alpha')
plt.ylabel('Mean square error')
plt.title('Mean square error pour chaque échantillon')
plt.legend();
```

La CV interne réutilise ici les données standardisées comme dans le support : cela protège le test final, mais la standardisation globale du train peut influencer les scores des plis. Pour une sélection rigoureuse, le prétraitement doit être réappris dans chaque pli ; les pipelines restent un approfondissement ultérieur.

## Lire les coefficients

```python
coefficients = pd.Series(modele.coef_, index=X.columns)
print(coefficients)
print("Constante :", modele.intercept_)



lasso_coef = lasso_reg.coef_

plt.plot(range(len(X.columns)), lasso_coef)
plt.xticks(range(len(X.columns)), X.columns.values, rotation=70);
```

L’ordre des coefficients suit les colonnes de X. Après standardisation, une unité représente un écart-type du train. Un signe positif ou négatif décrit l’association conditionnelle dans le modèle, pas une causalité.

# Évaluer le modèle

## R², MSE et RMSE

R² peut être négatif. MSE est une erreur au carré ; RMSE revient dans l’unité de la cible. Comparer les valeurs sur le même jeu de test.

```python
from sklearn.metrics import mean_squared_error

mse = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)
r2 = modele.score(X_test, y_test)
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
prediction_train = modele.predict(X_train)
print("RMSE train :", np.sqrt(mean_squared_error(y_train, prediction_train)))
print("RMSE test :", rmse)
```

Un grand écart peut signaler du surapprentissage, mais un seul découpage ne suffit pas à conclure.

## Paramètres utiles

- `alphas` : grille de pénalités positives ; `cv=10` : dix plis, à adapter à la taille des données.
- `max_iter` : plafond d’itérations d’optimisation.
- `Lasso(alpha=1)` correspond à l’essai à pénalité fixée du cours.

## Interprétation

`coef_` peut contenir des zéros ; `alpha_` est retenu par CV. `alphas_` et `mse_path_` décrivent les essais.

# Pattern minimal

Les matrices ci-dessous ont déjà été préparées avec le **même objet appris sur le train** ; elles ne désignent pas les données brutes. 

```python
from sklearn.linear_model import LassoCV

modele = LassoCV(alphas=[0.001, 0.01, 0.1, 1.0, 10.0], cv=10, max_iter=10000)
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

- [linear_model.LassoCV](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LassoCV.html)
- [linear_model.Lasso](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.Lasso.html)

