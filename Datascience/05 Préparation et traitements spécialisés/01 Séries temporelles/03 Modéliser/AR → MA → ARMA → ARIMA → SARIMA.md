# ARMA

# Workflow — Séries temporelles : AR → MA → ARMA → ARIMA → SARIMA

## Quand l’utiliser ?

Préparer une **série temporelle univariée**, examiner sa structure, puis **entraîner et comparer** successivement les modèles AR, MA, ARMA, ARIMA et SARIMA. Cette fiche est un **workflow de codage** ; les ordres choisis sont illustratifs, pas optimisés.

**Point de départ :** `airpasslog`, une `pd.Series` mensuelle déjà préparée, dont l'index est chronologique et les valeurs sont les **logarithmes** du nombre de passagers (comme dans l'exercice Liora). Adapter le nom et la fréquence à une autre série. Les prévisions obtenues ici sont **sur l'échelle logarithmique**.

## Principe — repères rapides

|Modèle|Ce qu’on ajoute|Code `statsmodels`|
|---|---|---|
|AR(p)|Valeurs passées|`ARIMA(order=(p, 0, 0))`|
|MA(q)|Innovations / erreurs passées|`ARIMA(order=(0, 0, q))`|
|ARMA(p, q)|AR + MA ; série stationnaire|`ARIMA(order=(p, 0, q))`|
|ARIMA(p, d, q)|Différenciation ordinaire `d`|`ARIMA(order=(p, d, q))`|
|SARIMA(p, d, q)(P, D, Q, s)|Différenciation et composantes saisonnières|`SARIMAX(order=..., seasonal_order=...)`|

**MA** = composante _moving average_ des **innovations**, et non moyenne glissante `rolling().mean()`.

---

# Pas à pas

## 1. Importer les bibliothèques

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

from pandas.plotting import autocorrelation_plot
from statsmodels.tsa.seasonal import seasonal_decompose
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf
from statsmodels.tsa.arima.model import ARIMA
from statsmodels.tsa.statespace.sarimax import SARIMAX
from sklearn.metrics import mean_absolute_error, mean_squared_error

df = pd.read_csv('airpass.csv', parse_dates=['Date'], index_col='Date')
df.head()
```

## 2. Vérifier la série et découper chronologiquement

```python
# airpasslog est déjà une Series mensuelle, triée, sans NaN
print(airpasslog.head())
print(airpasslog.index)
print(airpasslog.isna().sum())

airpasslog.plot(figsize=(10, 4), title="Série mensuelle (log)")
plt.show()

# Garder les 12 derniers mois pour le test
train = airpasslog.iloc[:-12]
test = airpasslog.iloc[-12:]
```

**À vérifier avant ce code :** dates triées, fréquence mensuelle régulière et absence de valeurs manquantes. Ne pas mélanger aléatoirement les observations. Les transformations destinées à choisir le modèle se font sur le **train**.
![[Pasted image 20261019104232.png]]
## 3. Observer tendance, saisonnalité et résidus

```python
decomposition = seasonal_decompose(train, model="additive", period=12)
decomposition.plot()
plt.show()
```

`period=12` correspond à une saisonnalité **annuelle** sur des observations **mensuelles**. Il faut au moins deux cycles complets. Cette décomposition est **descriptive**, pas une prévision.
![[Pasted image 20261019104241.png]]
## 4. Visualiser l'autocorrélation de la série initiale

```python
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(16, 5))
train.plot(ax=ax1, title="Série initiale (train)")
autocorrelation_plot(train, ax=ax2)
plt.show()

from statsmodels.tsa.stattools import adfuller

resultat = adfuller(serie.dropna())
print("Statistique ADF :", resultat[0])
print("p-value :", resultat[1])
```

**Lecture :** décroissance lente des corrélations → stationnarité à examiner ; pics aux retards `12, 24, 36…` → possible saisonnalité annuelle. L'ACF donne des **indices**, pas une preuve de stationnarité.
![[Pasted image 20261019104251.png]]
## 5. Tester une différenciation d'ordre 1

```python
train_diff1 = train.diff().dropna()

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(16, 5))
train_diff1.plot(ax=ax1, title="Différenciation d'ordre 1")
autocorrelation_plot(train_diff1, ax=ax2)
plt.show()

#ou exemple de cours
sales_diff=sales_log.diff().dropna()

pd.plotting.autocorrelation_plot(sales_diff); 
resultat = adfuller(sales_log.dropna())
print("Statistique ADF :", resultat[0])
print("p-value :", resultat[1])
```

`diff()` calcule `valeur du mois − valeur du mois précédent`. `dropna()` supprime la première observation, qui n'a pas de mois précédent. **On ne passe pas automatiquement à `d=1`** : on examine si la transformation est pertinente.

Si les pics aux lags 12, 24… persistent, cela suggère une composante saisonnière à prendre en compte.
![[Pasted image 20261019104302.png]]
## 6. Examiner ACF et PACF pour proposer des ordres

```python
fig, axes = plt.subplots(1, 2, figsize=(12, 4))
plot_acf(train_diff1, lags=36, ax=axes[0])
plot_pacf(train_diff1, lags=36, ax=axes[1], method="ywm")
plt.tight_layout()
plt.show()
```

L'**ACF** et la **PACF** orientent le choix de `p`, `q` et éventuellement des ordres saisonniers. Elles ne fixent pas automatiquement les meilleurs paramètres. Adapter `lags` à la taille du train.
![[Pasted image 20261019104314.png]]
## 7. AR : composante autorégressive seule

```python
# Exemple : AR(1) → p=1, d=0, q=0
res_ar = ARIMA(train, order=(1, 0, 0)).fit()
print(res_ar.summary())
```

**Attention :** ce modèle suppose que la série `train` est suffisamment stationnaire. Dans le cas d'AirPassengers, ce n'est probablement **pas** une bonne modélisation finale ; on l'exécute ici pour comprendre la syntaxe et comparer les familles.
![[Pasted image 20261019104329.png]]
## 8. MA : innovations retardées seules

```python
# Exemple : MA(1) → p=0, d=0, q=1
res_ma = ARIMA(train, order=(0, 0, 1)).fit()
print(res_ma.summary())
```

MA n'est pas un calcul de moyenne glissante ; c'est un modèle de série utilisant des **innovations passées**. Lui aussi suppose ici une série stationnaire.
![[Pasted image 20261019104341.png]]
## 9. ARMA : combiner AR et MA

```python
# Exemple : ARMA(1,1) → d=0
res_arma = ARIMA(train, order=(1, 0, 1)).fit()
print(res_arma.summary())
```

**Pour un usage réel**, AR, MA et ARMA s'ajustent à une série suffisamment stationnaire. On peut comparer leurs structures sur une série déjà stationnaire, mais il ne faut pas retenir aveuglément un modèle ajusté aux niveaux non stationnaires d'AirPassengers.
![[Pasted image 20261019104355.png]]
## 10. ARIMA : intégrer la différenciation ordinaire

```python
# Exemple : ARIMA(1,1,1) → différenciation intégrée d=1
res_arima = ARIMA(train, order=(1, 1, 1)).fit()
print(res_arima.summary())
```

**Ne pas passer `train_diff1` ici** : avec `d=1`, `ARIMA` réalise déjà la différenciation en interne. Les prévisions reviennent sur l'échelle d'entrée (`train`, ici logarithmique).
![[Pasted image 20261019104407.png]]
## 11. SARIMA : ajouter la structure saisonnière

```python
# Exemple illustratif pour une série mensuelle saisonnière
res_sarima = SARIMAX(
    train,
    order=(1, 1, 1),
    seasonal_order=(1, 1, 1, 12),
    enforce_stationarity=False,
    enforce_invertibility=False
).fit(disp=False)

print(res_sarima.summary())
```
![[Pasted image 20261019104417.png]]
- `order=(p, d, q)` : structure **non saisonnière**.
- `seasonal_order=(P, D, Q, s)` : structure **saisonnière** ; `s=12` pour un cycle annuel de données mensuelles.
- `D=1` introduit une différence avec la valeur **12 mois plus tôt** ; `d=1` une différence avec le **mois précédent**.

`enforce_stationarity=False` et `enforce_invertibility=False` assouplissent des contraintes numériques du modèle ; **cela ne rend pas inutile l'examen de la stationnarité**.

## 12. Produire les prévisions sur le jeu test

```python
# Exemples : comparer ARIMA et SARIMA sur les mêmes 12 mois
pred_arima = res_arima.forecast(steps=len(test))
pred_sarima = res_sarima.forecast(steps=len(test))

print("Dates ARIMA alignées :", pred_arima.index.equals(test.index))
print("Dates SARIMA alignées :", pred_sarima.index.equals(test.index))

ax = train.plot(figsize=(12, 5), label="Train")
test.plot(ax=ax, label="Test")
pred_arima.plot(ax=ax, label="ARIMA")
pred_sarima.plot(ax=ax, label="SARIMA")
plt.legend()
plt.show()
```

**Comparer les prévisions avec le test sur le même horizon et la même échelle.** Si l'index de prévision n'est pas aligné avec celui du test, vérifier la fréquence des dates plutôt que forcer silencieusement l'alignement.
![[Pasted image 20261019104429.png]]
---

# Évaluer le modèle

## MAE et RMSE

```python
def evaluer(y_reel, y_pred):
    mae = mean_absolute_error(y_reel, y_pred)
    rmse = np.sqrt(mean_squared_error(y_reel, y_pred))
    return {"MAE": mae, "RMSE": rmse}

print("ARIMA :", evaluer(test, pred_arima))
print("SARIMA :", evaluer(test, pred_sarima))
```

Ici les scores concernent des **logarithmes**, puisque `airpasslog` est logarithmique. Pour exprimer des prévisions en nombre de passagers, utiliser `np.exp(pred_sarima)` et comparer avec les valeurs réelles **sur l'échelle originale** ; l'exponentielle d'une prévision logarithmique ne corrige pas automatiquement le biais de retransformation.
![[Pasted image 20261019104440.png]]
## Diagnostiquer les résidus

```python
res_sarima.plot_diagnostics(figsize=(12, 7))
plt.tight_layout()
plt.show()

# Autocorrélation des résidus
plot_acf(res_sarima.resid.dropna(), lags=24)
plt.show()
```

Chercher des résidus sans structure temporelle évidente. Des pics d'autocorrélation persistants signalent une dépendance que le modèle n'a pas captée. Les premières erreurs d'initialisation du modèle peuvent perturber ce diagnostic.
![[Pasted image 20261019104455.png]]
## Comparer les modèles

```python
print("AIC ARIMA :", res_arima.aic)
print("AIC SARIMA :", res_sarima.aic)
```

L'AIC peut servir à départager des candidats ajustés sur **les mêmes données**, mais le choix final doit aussi considérer l'erreur sur une période future non utilisée pour les réglages. Avec un seul découpage train/test, ne pas sélectionner indéfiniment les ordres sur le test final.
![[Pasted image 20261019104506.png]]
---

# Pattern minimal

Pour une **série mensuelle** déjà préparée et un modèle saisonnier retenu sur l'apprentissage :

```python
from statsmodels.tsa.statespace.sarimax import SARIMAX

train = serie.iloc[:-12]
test = serie.iloc[-12:]

modele = SARIMAX(
    train,
    order=(p, d, q),
    seasonal_order=(P, D, Q, 12)
)
resultats = modele.fit(disp=False)
previsions = resultats.forecast(steps=len(test))
```

Pour les modèles précédents, remplacer l'instanciation :

```
# AR      : ARIMA(train, order=(p, 0, 0))
# MA      : ARIMA(train, order=(0, 0, q))
# ARMA    : ARIMA(train, order=(p, 0, q))
# ARIMA   : ARIMA(train, order=(p, d, q))
```

## Points de vigilance

- **Ordres illustratifs** `(1, 1, 1)` : ni valeurs imposées par le cours, ni choix optimal garanti.
- Toujours conserver **l'ordre chronologique** des observations et isoler les derniers mois pour l'évaluation.
- Préparer et inspecter la série **sur le train** avant de sélectionner les réglages.
- `.diff()` sert à **observer** une transformation ; si le modèle comporte `d=1`, ne pas fournir en plus une série déjà différenciée.
- Une série peut être **stationnaire et saisonnière** : les pics saisonniers seuls ne prouvent pas la non-stationnarité.
- Le `s` de SARIMA correspond au **nombre de périodes par saison**, pas systématiquement à 12.
- Les résidus de `seasonal_decompose()` et les résidus d'un modèle prédictif ne sont pas interchangeables.
- `SARIMAX` peut également accepter des variables externes avec `exog=...`, mais cela sort de ce workflow univarié **SARIMA**.

## Références

- Notions et noms de variables suivis : exercice Liora sur `airpasslog`, différenciation, autocorrélation et modèles AR à SARIMA, à partir des extraits discutés. Les codes d'estimation comparée constituent ici un **workflow de référence**, sans prétendre reproduire ligne par ligne un notebook Liora non fourni.
- [statsmodels — ARIMA](https://www.statsmodels.org/stable/generated/statsmodels.tsa.arima.model.ARIMA.html)
- [statsmodels — SARIMAX](https://www.statsmodels.org/stable/generated/statsmodels.tsa.statespace.sarimax.SARIMAX.html)
- [pandas — Series.diff](https://pandas.pydata.org/docs/reference/api/pandas.Series.diff.html)

## Optimisation 
```python
import itertools
import warnings
warnings.filterwarnings('ignore')

d, D, s = 1, 1, 12
p_range = range(0, 5)
q_range = range(0, 4)
P_range = range(0, 4)
Q_range = range(0, 2)

param_combinations = list(itertools.product(p_range, q_range, P_range, Q_range))

best_bic = float('inf')
best_order = None

for p, q, P, Q in param_combinations:
    print(f"Testing SARIMA({p}, {d}, {q}) x ({P}, {D}, {Q}, {s})...")
    model = SARIMAX(sales_log, order=(p, d, q), seasonal_order=(P, D, Q, s))
    model_fit = model.fit(disp=False)

    if model_fit.bic < best_bic:
        best_bic = model_fit.bic
        best_order = (p, q, P, Q)

print(f"Meilleur modèle trouvé : (p={best_order[0]}, d={d}, q={best_order[1]}) x (P={best_order[2]}, D={D}, Q={best_order[3]}, s={s})")
print(f"BIC du meilleur modèle : {best_bic}")
 ```