# Ridge

## Quand l’utiliser ?

Prédire une cible quantitative en régularisant un modèle linéaire. Comparer avec la régression linéaire non pénalisée.

## Principe

Une pénalité L2 réduit l’amplitude des coefficients ; RidgeCV choisit sa force par validation interne.

"ajout d'une contrainte sur les coefficients lors de la modélisation pour maîtriser l'amplitude de leurs valeurs"  fonction à minimiser : min𝛽(||𝑌−𝑋𝛽||2+𝛼||𝛽||2)

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
from sklearn.linear_model import RidgeCV

modele = RidgeCV(alphas=[0.001, 0.01, 0.1, 1.0, 10.0, 100.0])
modele.fit(X_train, y_train)
y_pred = modele.predict(X_test)
```

## Lire le réglage retenu

```python
alpha_retenu = modele.alpha_
print("Alpha retenu :", alpha_retenu)
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
prediction_train = modele.predict(X_train_modele)
print("RMSE train :", np.sqrt(mean_squared_error(y_train, prediction_train)))
print("RMSE test :", rmse)
```

Un grand écart peut signaler du surapprentissage, mais un seul découpage ne suffit pas à conclure.

## Paramètres utiles

- `alphas` : valeurs strictement positives à comparer.
- `cv=None` : validation leave-one-out efficace de RidgeCV ; un entier choisit un nombre de plis.
- `Ridge(alpha=...)` permet un réglage fixé, sans sélection interne.

## Interprétation

Les coefficients sont contractés, généralement sans être annulés. `alpha_` contient la valeur sélectionnée.

# Pattern minimal

Les matrices ci-dessous ont déjà été préparées avec le **même objet appris sur le train** ; elles ne désignent pas les données brutes. 

```python
from sklearn.linear_model import RidgeCV

modele = RidgeCV(alphas=[0.001, 0.01, 0.1, 1.0, 10.0, 100.0])
modele.fit(X_train_modele, y_train)
y_pred = modele.predict(X_test_modele)
```



## Points de vigilance

- Standardiser les variables pour rendre la pénalité comparable.
- Avec une validation croisée, le prétraitement doit être réappris dans chaque pli. Standardiser une fois tout le train avant la CV laisse les plis de validation influencer le prétraitement ; le test final reste séparé, mais le score de CV peut être optimiste. Les pipelines seront traités ultérieurement.
- Ridge réduit les coefficients mais ne constitue pas une sélection explicite de variables.


## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de regression.md`.

- [linear_model.RidgeCV](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.RidgeCV.html)
- [linear_model.Ridge](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.Ridge.html)

