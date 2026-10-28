# Régression linéaire

## Quand l’utiliser ?

Prédire une cible quantitative à partir d’une variable. Cours : prix des automobiles, puis points marqués en NBA.

## Principe

LinearRegression estime une combinaison linéaire des variables en minimisant la somme des erreurs au carré. Simple et multiple utilisent la même classe.

> Exemple automobiles.csv : prédire price. Pour les alternatives non linéaires, le déploiement est une adaptation des classes citées dans les compléments du cours.

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

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

# Simple : une seule variable, conservée en 2D avec doubles crochets.
slr = LinearRegression()
slr.fit(X_train, y_train)
y_pred = slr.predict(X_test)

print("R2 simple :", slr.score(X_test, y_test))

```

## Lire les coefficients

```python
print(slr.coef_)
print("Constante :", slr.intercept_)
```

L’ordre des coefficients suit les colonnes de X. Après standardisation, une unité représente un écart-type du train. Un signe positif ou négatif décrit l’association conditionnelle dans le modèle, pas une causalité.

# 4.Évaluer le modèle

## 4.1 R², MSE et RMSE : est-ce que je prédis correctement?

R² (coefficient de détermination) peut être négatif. MSE est une erreur au carré ; RMSE revient dans l’unité de la cible. Comparer les valeurs sur le même jeu de test.

```python
from sklearn.metrics import mean_squared_error

mse = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)  #rmse = root_mean_squared_error(y_test, y_pred)
r2 = slr.score(X_test, y_test)
print({"R2": r2, "MSE": mse, "RMSE": rmse})
```
avec R²=0.75 on peut lire le modèle explique 75% de la variabilité observée de y

## 4.2 Examiner les résidus : Mes erreurs présentent elles une structure?
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

## 4.3 QQplot : la distribution de mes résidus ressemble-t-elle suffisamment à une distribution normale

```python
import scipy.stats as stats

residus_norm = (residus-residus.mean())/residus.std()
stats.probplot(residus_norm, plot=plt);
```

## 4.4 Comparer apprentissage et test : est-ce qu'il reste bon sur des données qu'il n'a pas utilisées pour apprendre ?

```python
prediction_train = slr.predict(X_train)
print("RMSE train :", np.sqrt(mean_squared_error(y_train, prediction_train)))
print("RMSE test :", rmse)
```

Un grand écart peut signaler du surapprentissage, mais un seul découpage ne suffit pas à conclure.

             ÉVALUER LE MODÈLE
                    │
       ┌────────────┼─────────────┐
       ↓            ↓             ↓
   PERFORMANCE   DIAGNOSTIC   GÉNÉRALISATION
       │            │             │
 R² / MSE / RMSE  résidus      train vs test
                    │                                                                                                                                                                                                                                                                                                                        
              ┌─────┴─────┐
              ↓           ↓
         résidus/y_pred  QQ-plot
## 
Paramètres utiles

- `X_train`/`X_test` : DataFrames ; `"mp"` est le nombre de minutes du jeu NBA, à remplacer par la colonne étudiée.
- `fit_intercept=True` : estime une constante ; ne pas le désactiver sans raison.
- `coef_` : un coefficient par variable, dans l’ordre des colonnes ; `intercept_` : constante.

## Interprétation

β₁ représente la variation de la valeur prédite de y lorsque X augmente d'une unité..

ŷ = β₀ + β₁X

β₀ → ordonnée à l'origine
β₁ → pente


## Points de vigilance

- Ne pas confondre variable cible et colonne explicative ; ne pas garder dans X une information qui révèle directement y.
- Valeurs aberrantes, relations non linéaires et colinéarité peuvent rendre le modèle fragile.
- Les hypothèses utilisées pour l’inférence statistique ne sont pas toutes des conditions nécessaires pour appeler fit.
- Le signe choisi ici pour les résidus est réel − prédit ; rester cohérente dans les graphiques.


## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de regression.md`.

- [linear_model.LinearRegression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)

