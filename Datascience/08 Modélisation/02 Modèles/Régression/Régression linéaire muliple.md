# Régression linéaire

## Quand l’utiliser ?

Prédire une cible quantitative à partir de plusieurs variables. 

## Principe

LinearRegression estime une combinaison linéaire des variables en minimisant la somme des erreurs au carré. Simple et multiple utilisent la même classe.

> Exemple automobiles.csv : prédire price. Pour les alternatives non linéaires, le déploiement est une adaptation des classes citées dans les compléments du cours.


---

# Pas à pas

## 1. Préparer les données et définir X / y

Le fichier du cours contient déjà les caractéristiques numériques. La cible price ne doit pas rester dans X. La suppression de lignes incomplètes est un choix illustratif, pas une règle générale.

## 2. Séparer train et test

`X` est une matrice 2D, `y` un vecteur 1D. Les quatre sorties restent alignées. Le train sert à apprendre ; le test reste à l’écart du choix des réglages. En classification, on peut ajouter `stratify=y` si les effectifs le permettent ; le découpage ci-dessous reprend celui du cours.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=789
)
print(X_train.shape, X_test.shape, y_train.shape, y_test.shape)
```

## 3. Instancier, entraîner et prédire

Le modèle apprend une cible quantitative. predict retourne des nombres, jamais des probabilités de classes.

```python
from sklearn.linear_model import LinearRegression

# Multiple : toutes les caractéristiques numériques retenues.
mlr = LinearRegression()
mlr.fit(X_train, y_train)
y_pred = mlr.predict(X_test)
```

## Lire les coefficients

```python
coefficients = pd.Series(mlr.coef_, index=X.columns)
print(coefficients)
print("Constante :", mlr.intercept_)
```

L’ordre des coefficients suit les colonnes de X. Après standardisation, une unité représente un écart-type du train. Un signe positif ou négatif décrit l’association conditionnelle dans le modèle, pas une causalité.

# 4.Évaluer le modèle

## R², MSE et RMSE : est-ce que je prédis correctement?

R² (coefficient de détermination) peut être négatif. MSE est une erreur au carré ; RMSE revient dans l’unité de la cible. Comparer les valeurs sur le même jeu de test.

```python
from sklearn.metrics import mean_squared_error

mse = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)
r2 = mlr.score(X_test, y_test)

#possibilité de comparéer mlr.score(X_train, y_train) : à quel point le modèle ajuste-il les données qu'il a utilisé et à quel point fonctionne-t-il sur les données jamais vus?

print({"R2": r2, "MSE": mse, "RMSE": rmse})


```
avec R²=0.75 on peut lire le modèle explique 75% de la variabilité observée de y


## Examiner les résidus : Mes erreurs présentent elles une structure?
résidus : comment mon modèle se trompe-t-il?
Convention : réel − prédit. Un motif courbe, un éventail ou quelques erreurs extrêmes orientent les vérifications.

```python
residus = y_test - y_pred #dansle cours différence entre la valeur ajustée et la valeur réelle soit y_pred - y_test

plt.scatter(y_pred, residus)
plt.axhline(0, color="black", linestyle="--")
plt.xlabel("Valeurs prédites")
plt.ylabel("Résidus : réel - prédit")
plt.show()
```
y_pred - y_train ==> diagnostic du modèle ajusté : j'ai ajusté ma croite/hyperplan sur le train. Regardons comment cet ajustement se comporte sur ces observations.

y_test - y_pred : ==> diagnostic performance predictive et généralisation : sur les données jamais vues, quelles erreurs jamais vues, quelles erreurs fait réellement mon modèle

## QQplot : la distribution de mes résidus ressemble-t-elle suffisamment à une distribution normale

```python
import scipy.stats as stats

residus_norm = (residus-residus.mean())/residus.std()
stats.probplot(residus_norm, plot=plt);
```

## Comparer apprentissage et test : est-ce qu'il reste bon sur des données qu'il n'a pas utilisées pour apprendre ?

```python
y_pred_train = mlr.predict(X_train)
print("RMSE train :", np.sqrt(mean_squared_error(y_train, y_pred_train)))
print("RMSE test :", rmse)
```


# 5. Affiner le modèle
“baseline → diagnostic → affinage → nouveau modèle → comparaison”
### 5.1 par SelectKBest
```python
#Selection des meilleurs **scores F univariés de régression**
from sklearn.feature_selection import SelectKBest
from sklearn.feature_selection import f_regression

sk = SelectKBest(f_regression, k=3)

sk.fit(X_train, y_train)

X.columns[sk.get_support()]

#création des tableaux sk_train et sk_test à partir de X_train et X_test
sk_train = sk.transform(X_train)
sk_test = sk.transform(X_test)

#Création du modele et affichage du score
sklr = LinearRegression()
sklr.fit(sk_train, y_train)

print(sklr.score(sk_train, y_train))
print(sklr.score(sk_test, y_test))
```

### 5.2 par SelectFromModel
```Python

#Selection des variables avec le plus de poids dans la prédiction
from sklearn.feature_selection import SelectFromModel

lr=LinearRegression()
sfm=SelectFromModel(lr)
scaler = preprocessing.StandardScaler().fit(X_train)

#Création des tableaux 
X_train_scaled=scaler.transform(X_train)
X_test_scaled=scaler.transform(X_test)

sfm_train = sfm.fit_transform(X_train_scaled, y_train)
sfm_test = sfm.transform(X_test_scaled)
X.columns[sfm.get_support()]


#Création du modele et affichage du score
sfmlr = LinearRegression()
sfmlr.fit(sfm_train, y_train)

print(sfmlr.score(sfm_train, y_train))
print(sfmlr.score(sfm_test, y_test))
```

## Paramètres utiles

- `X_train`/`X_test` : DataFrames ; `"mp"` est le nombre de minutes du jeu NBA, à remplacer par la colonne étudiée.
- `fit_intercept=True` : estime une constante ; ne pas le désactiver sans raison.
- `coef_` : un coefficient par variable, dans l’ordre des colonnes ; `intercept_` : constante.

## Interprétation

Un coefficient décrit une variation de la prédiction pour une unité supplémentaire, les autres variables étant fixées. Sur des données standardisées, l’unité est un écart-type du train. Cela ne prouve pas une causalité.

# Pattern minimal

Les matrices ci-dessous ont déjà été préparées avec le **même objet appris sur le train** ; elles ne désignent pas les données brutes. 

```python
from sklearn.linear_model import LinearRegression

modele = LinearRegression()
modele.fit(X_train_modele, y_train)
y_pred = modele.predict(X_test_modele)
```



## Points de vigilance

- Ne pas confondre variable cible et colonne explicative ; ne pas garder dans X une information qui révèle directement y.
- Valeurs aberrantes, relations non linéaires et colinéarité peuvent rendre le modèle fragile.
- Les hypothèses utilisées pour l’inférence statistique ne sont pas toutes des conditions nécessaires pour appeler fit.
- Le signe choisi ici pour les résidus est réel − prédit ; rester cohérente dans les graphiques.


## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de regression.md`.

- [linear_model.LinearRegression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)

