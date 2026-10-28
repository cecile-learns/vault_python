
# Workflow — Séries temporelles : SARIMAX

## Quand l’utiliser ?

Prévoir une **série temporelle univariée** en tenant compte :

- De ses valeurs passées (AR).
    
- De ses innovations passées (MA).
    
- De sa tendance et de sa saisonnalité (différenciations et composantes saisonnières).
    
- De **variables explicatives externes** susceptibles d'améliorer la prédiction.
    

**Exemple Compagnon Immobilier :** prévoir le prix médian au m² par trimestre en tenant compte de son historique, de la saisonnalité éventuelle et d'indicateurs immobiliers ou territoriaux.

**Point de départ :**

- `df`, un DataFrame chronologique, à fréquence trimestrielle régulière.
    
- `prix_m2`, la variable cible.
    
- Des variables explicatives numériques, par exemple `revenu_median` et `part_appartements`.
    

Ce workflow utilise des variables illustratives : leur disponibilité et leur pertinence devront être vérifiées avant toute application au projet.


**SARIMAX = Seasonal AutoRegressive Integrated Moving Average with eXogenous regressors.**

Le `X` représente les variables exogènes.

Le modèle conserve la structure de SARIMA et lui ajoute une composante de régression sur les variables externes.

**Différence avec VAR :** SARIMAX prédit une cible principale à l'aide de son historique et de variables explicatives. VAR modélise simultanément plusieurs séries endogènes.

---

# Pas à pas

## 1. Importer les bibliothèques

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

from statsmodels.tsa.statespace.sarimax import SARIMAX
from statsmodels.tsa.stattools import adfuller
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf

from sklearn.metrics import (
    mean_absolute_error,
    mean_absolute_percentage_error,
    root_mean_squared_error
)
```

## 2. Préparer les données

```python
df = pd.read_csv(
    "donnees.csv",
    parse_dates=["Date"],
    index_col="Date"
)

df = df.sort_index()

# Exemple : agrégation trimestrielle
df = df.resample("QE").agg({
    "prix_m2": "median",
    "revenu_median": "mean",
    "part_appartements": "mean"
})

print(df.head())
print(df.isna().sum())
```

**Attention :** les fonctions d'agrégation dépendent de la signification des variables.

- `median()` : médiane des observations.
    
- `mean()` : moyenne.
    
- `sum()` : somme des observations.
    

Ne pas agréger mécaniquement toutes les colonnes avec la même fonction.

Pour Compagnon Immobilier, si `prix_m2` représente déjà une médiane calculée par période, il faut éviter de recalculer une médiane de médianes sans justification.

## 3. Définir la cible et les variables exogènes

```python
y = df["prix_m2"]

X = df[
    ["revenu_median", "part_appartements"]
]
```

- `y` : variable à prédire, ici le prix médian au m².
    
- `X` : variables explicatives externes.
    

Chaque ligne de `X` doit correspondre à la même période que la ligne de `y`.

**Important :** les variables exogènes ne sont pas nécessairement indépendantes statistiquement de la cible. Le terme désigne ici les variables fournies au modèle, dont la dynamique n'est pas modélisée simultanément.

## 4. Séparer chronologiquement les données

```python
# Exemple : quatre derniers trimestres en test
y_train = y.iloc[:-4]
y_test = y.iloc[-4:]

X_train = X.iloc[:-4]
X_test = X.iloc[-4:]
```

Vérifier l'alignement :

```python
print(y_train.index.equals(X_train.index))
print(y_test.index.equals(X_test.index))
```

Les dates doivent correspondre.

**Pas de** `**train_test_split**` **aléatoire** : les données futures ne doivent pas servir à entraîner le modèle.

Le test doit rester indépendant du choix des paramètres.

## 5. Examiner la stationnarité

Comme pour SARIMA, on commence par étudier la série cible.

```python
y_train.plot(figsize=(10, 4))
plt.title("Série temporelle — train")
plt.show()

resultat = adfuller(y_train.dropna())

print("Statistique ADF :", resultat[0])
print("p-value :", resultat[1])
```

Le test ADF examine l'hypothèse de racine unitaire :

- `p-value < 0.05` : on rejette cette hypothèse.
    
- `p-value >= 0.05` : on ne dispose pas de suffisamment d'éléments pour la rejeter.
    

Le graphique, les autocorrélations et le contexte métier doivent compléter ce test.

## 6. Examiner les différenciations et les autocorrélations

```python
# Différenciation ordinaire
y_diff = y_train.diff().dropna()

# Exemple de différenciation saisonnière
# Série trimestrielle, cycle annuel de 4 trimestres
y_diff_saison = y_diff.diff(periods=4).dropna()

fig, axes = plt.subplots(1, 2, figsize=(12, 4))

plot_acf(y_diff_saison, ax=axes[0], lags=12)
plot_pacf(y_diff_saison, ax=axes[1], lags=12)

plt.tight_layout()
plt.show()
```

Le choix des ordres suit la logique étudiée dans [[AR → MA → ARMA → ARIMA → SARIMA]].

|Paramètre|Outil principal|
|---|---|
|`d`|Différenciation ordinaire, graphique, ADF|
|`D`|Différenciation saisonnière, graphique|
|`s`|Périodicité métier et structure saisonnière|
|`p`|PACF, lags ordinaires|
|`q`|ACF, lags ordinaires|
|`P`|PACF, lags saisonniers|
|`Q`|ACF, lags saisonniers|

**Les graphiques ACF/PACF suggèrent des ordres candidats, mais ne garantissent pas le meilleur modèle.**

Pour des données trimestrielles avec saisonnalité annuelle, `s=4`. Pour des données mensuelles avec saisonnalité annuelle, `s=12`.

Les différenciations servent ici au diagnostic. Le modèle appliquera ensuite automatiquement celles correspondant à `d` et `D`.

## 7. Instancier un modèle SARIMAX

```python
# Ordres illustratifs, non optimisés
model = SARIMAX(
    y_train,
    exog=X_train,
    order=(1, 1, 1),
    seasonal_order=(1, 1, 1, 4)
)
```

- `y_train` : cible historique.
    
- `exog=X_train` : variables externes historiques.
    
- `order=(p,d,q)` : composantes non saisonnières.
    
- `seasonal_order=(P,D,Q,s)` : composantes saisonnières.
    

L'exemple suppose une série trimestrielle avec un cycle annuel. Les valeurs des ordres doivent être déterminées selon les données.

**Ne pas fournir** `**y_diff_saison**` **au modèle si** `**d=1**` **et** `**D=1**` : SARIMAX effectue déjà ces différenciations.

## 8. Ajuster le modèle

```
results = model.fit(disp=False)

print(results.summary())
```

Le résumé contient notamment :

- Les coefficients AR et MA.
    
- Les coefficients saisonniers.
    
- Les coefficients associés aux variables exogènes.
    
- Leurs erreurs standards et p-values.
    
- Les critères AIC et BIC.
    
- Des diagnostics sur les résidus.
    

Un coefficient exogène positif indique une association positive estimée avec la cible, **à caractéristiques du modèle données**. Il ne démontre pas une relation causale.

Une p-value élevée ne suffit pas, à elle seule, à décider de supprimer systématiquement une variable.

## 9. Produire les prévisions

```
prediction = results.get_forecast(
    steps=len(y_test),
    exog=X_test
)

forecast_df = prediction.summary_frame()

forecast_df
```

Les principales colonnes du résultat sont :

- `mean` : prévision centrale.
    
- `mean_se` : erreur standard de prévision.
    
- `mean_ci_lower` : borne inférieure de l'intervalle.
    
- `mean_ci_upper` : borne supérieure de l'intervalle.
    

**Particularité de SARIMAX :** il faut fournir les variables exogènes pour les dates futures.

Dans cet exemple, `X_test` contient les valeurs des variables explicatives sur la période de test.

Pour une véritable prévision future, les variables exogènes devront être connues ou prédites au moment où l'on réalise la prévision.

## 10. Visualiser les prévisions

```
plt.figure(figsize=(12, 5))

plt.plot(
    y_train.index,
    y_train,
    label="Train"
)

plt.plot(
    y_test.index,
    y_test,
    label="Valeurs réelles"
)

plt.plot(
    forecast_df.index,
    forecast_df["mean"],
    label="Prédictions",
    linestyle="--"
)

plt.fill_between(
    forecast_df.index,
    forecast_df["mean_ci_lower"],
    forecast_df["mean_ci_upper"],
    alpha=0.2,
    label="Intervalle de prévision"
)

plt.legend()
plt.title("Prévisions SARIMAX")
plt.show()
```

Rechercher :

- Les biais de surestimation ou sous-estimation.
    
- Les changements de tendance mal anticipés.
    
- Les effets saisonniers non capturés.
    
- L'élargissement de l'incertitude avec l'horizon.
    

---

# Évaluer le modèle

## 11. Calculer MAE, RMSE et MAPE

```
y_pred = forecast_df["mean"]

mae = mean_absolute_error(y_test, y_pred)

rmse = root_mean_squared_error(y_test, y_pred)

mape = mean_absolute_percentage_error(y_test, y_pred)

print("MAE :", mae)
print("RMSE :", rmse)
print("MAPE :", mape * 100, "%")
```

- **MAE** : écart absolu moyen, exprimé dans l'unité de la cible.
    
- **RMSE** : pénalise davantage les grandes erreurs.
    
- **MAPE** : écart relatif moyen en pourcentage ; à éviter ou interpréter avec prudence lorsque les valeurs réelles sont nulles ou très proches de zéro.
    

Dans Compagnon Immobilier, si `y` est un prix en €/m², la MAE et la RMSE sont également exprimées en €/m².

## 12. Diagnostiquer les résidus

```
results.plot_diagnostics(figsize=(12, 7))

plt.tight_layout()
plt.show()
```

Puis, si nécessaire :

```
plot_acf(results.resid.dropna(), lags=12)
plt.show()
```

Les résidus doivent idéalement ne plus présenter de dépendance temporelle exploitable.

Il faut notamment examiner :

- L'autocorrélation résiduelle.
    
- La stabilité des erreurs.
    
- Les observations atypiques.
    
- Les éventuels problèmes de convergence.
    

Le test de Ljung-Box peut compléter cette vérification.

## 13. Comparer SARIMA et SARIMAX

L'intérêt des variables exogènes doit être évalué par rapport à un modèle sans ces variables.

```
# SARIMA : sans variables exogènes
model_sarima = SARIMAX(
    y_train,
    order=(1, 1, 1),
    seasonal_order=(1, 1, 1, 4)
)

results_sarima = model_sarima.fit(disp=False)

# SARIMAX : modèle avec exogènes
results_sarimax = results

print("BIC SARIMA :", results_sarima.bic)
print("BIC SARIMAX :", results_sarimax.bic)
```

Il est également important de comparer les erreurs de prévision sur les mêmes périodes :

```
pred_sarima = results_sarima.forecast(
    steps=len(y_test)
)

pred_sarimax = results_sarimax.get_forecast(
    steps=len(y_test),
    exog=X_test
).predicted_mean

print(
    "MAE SARIMA :",
    mean_absolute_error(y_test, pred_sarima)
)

print(
    "MAE SARIMAX :",
    mean_absolute_error(y_test, pred_sarimax)
)
```

**Objectif :** vérifier si l'ajout des variables explicatives améliore réellement les prévisions.

Un modèle plus complexe n'est pas automatiquement meilleur.

---

# Optimisation

## 14. Tester plusieurs combinaisons d'ordres

Comme pour SARIMA, on peut comparer différentes configurations sur le jeu d'entraînement.

```
import itertools
import warnings

parametres = itertools.product(
    [0, 1, 2],  # p
    [0, 1, 2],  # q
    [0, 1],     # P
    [0, 1]      # Q
)

resultats = []

with warnings.catch_warnings():
    warnings.simplefilter("ignore")

    for p, q, P, Q in parametres:
        try:
            model = SARIMAX(
                y_train,
                exog=X_train,
                order=(p, 1, q),
                seasonal_order=(P, 1, Q, 4)
            )

            fit = model.fit(disp=False)

            if fit.mle_retvals.get("converged", False):
                resultats.append({
                    "p": p,
                    "q": q,
                    "P": P,
                    "Q": Q,
                    "BIC": fit.bic
                })

        except (ValueError, np.linalg.LinAlgError):
            continue

classement = pd.DataFrame(resultats)

classement = classement.sort_values("BIC")

print(classement.head())
```

Cette grille est illustrative. Elle maintient `d=1`, `D=1` et `s=4` fixes pour concentrer la comparaison sur les ordres AR et MA.

Le BIC sélectionne le meilleur compromis parmi les configurations testées. Il ne remplace pas l'évaluation temporelle sur une période future.

**Attention :** avec des séries courtes, comme quelques années de prix immobiliers trimestriels, les modèles saisonniers complexes peuvent être difficiles à estimer de manière fiable.

---

# Pattern minimal

Pour une cible temporelle `y` et des variables exogènes numériques `X`, déjà préparées et alignées :

```
from statsmodels.tsa.statespace.sarimax import SARIMAX

# Séparation chronologique
y_train = y.iloc[:-4]
y_test = y.iloc[-4:]

X_train = X.iloc[:-4]
X_test = X.iloc[-4:]

# Modèle SARIMAX
model = SARIMAX(
    y_train,
    exog=X_train,
    order=(p, d, q),
    seasonal_order=(P, D, Q, s)
)

results = model.fit(disp=False)

# Prévisions
forecast = results.get_forecast(
    steps=len(y_test),
    exog=X_test
)

forecast_df = forecast.summary_frame()
```

Pour passer à un SARIMA sans variables exogènes, supprimer `exog=X_train` à l'entraînement et `exog=X_test` à la prévision.

---

## Points de vigilance

- **SARIMAX prédit une cible principale** : les variables exogènes ne sont pas prédites simultanément par ce modèle.
    
- Les valeurs futures des variables exogènes doivent être disponibles ou estimées pour chaque période prévue.
    
- `X_train` et `X_test` doivent conserver les **mêmes colonnes, dans le même ordre**.
    
- L'index temporel doit être régulier et correctement aligné.
    
- Les transformations et choix des paramètres doivent être établis sans utiliser le test final.
    
- La saisonnalité n'est pas obligatoire : il est inutile d'imposer `s=4` ou `s=12` si les données ne présentent pas de saisonnalité pertinente.
    
- Les ordres `p,d,q,P,D,Q` doivent être justifiés et comparés, pas copiés mécaniquement.
    
- Une variable explicative corrélée avec la cible n'est pas nécessairement utile à la prévision.
    
- Les données de démographie ou de logement peuvent être publiées avec retard ; leur disponibilité réelle doit être prise en compte.
    
- Une variable calculée à partir des transactions futures ne peut pas servir directement à prévoir ces mêmes transactions sans hypothèse supplémentaire.
    
- L'ajout de variables exogènes peut améliorer ou dégrader les performances. Il faut le mesurer.
    
- Si une transformation logarithmique a été appliquée à la cible, les prévisions doivent être ramenées à l'échelle originale pour l'interprétation métier, avec attention au biais de retransformation.
    

## Références

- Support Liora — _Time series : Analyses et prédictions_, parties **Entraînement d'un modèle SARIMA**, **Études de cas** et **Variables Exogènes**.
    
- [[AR → MA → ARMA → ARIMA → SARIMA]]
    
- [[05 Préparation et traitements spécialisés/01 Séries temporelles/03 Modéliser/VAR → VARMA → VARIMA|Workflow VAR → VARMA → VARIMA]]
    
- [statsmodels — SARIMAX](https://www.statsmodels.org/stable/generated/statsmodels.tsa.statespace.sarimax.SARIMAX.html)
    
- [statsmodels — SARIMAXResults.get_forecast](https://www.statsmodels.org/stable/generated/statsmodels.tsa.statespace.sarimax.SARIMAXResults.get_forecast.html)
    
- [statsmodels — SARIMAX FAQ](https://www.statsmodels.org/stable/examples/notebooks/generated/statespace_sarimax_faq.html)
    

#séries_temporelles #SARIMAX #machine_learning #workflow