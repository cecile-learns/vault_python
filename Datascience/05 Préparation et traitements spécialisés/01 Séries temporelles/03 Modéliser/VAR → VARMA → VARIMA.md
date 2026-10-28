
# Workflow — Séries temporelles : VAR → VARMA → VARIMA

## Quand l’utiliser ?

Modéliser et prévoir **plusieurs séries temporelles simultanément**, lorsque leurs évolutions peuvent être liées.

Cette fiche présente un **workflow de codage VAR**. Les modèles VARMA et VARIMA sont des extensions abordées conceptuellement dans le cours, mais sans procédure complète d'entraînement.

## Principe — repères rapides

|Modèle|Ce qu'on modélise|Particularité|
|---|---|---|
|**VAR(p)**|Valeurs passées de toutes les séries|Extension multivariée de AR|
|**VARMA(p,q)**|Valeurs passées et innovations passées de toutes les séries|Extension multivariée de ARMA|
|**VARIMA(p,d,q)**|VARMA avec différenciation|Adapté aux séries nécessitant une différenciation|

**Différence avec ARIMA :** on ne prédit plus une seule variable à partir de son passé, mais plusieurs variables en utilisant leurs historiques respectifs.

**Attention :** un VAR standard suppose des séries stationnaires. VARMA impose également un cadre stationnaire ; VARIMA introduit la différenciation pour traiter certaines formes de non-stationnarité.

---

# Pas à pas

## 1. Importer les bibliothèques

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from statsmodels.tsa.api import VAR
from statsmodels.tsa.stattools import adfuller

from sklearn.metrics import (
    mean_absolute_error,
    mean_absolute_percentage_error,
    r2_score
)
```

## 2. Préparer les données multivariées

```python
sales = pd.read_csv(
    "sales.csv",
    parse_dates=["Date"]
)

# Ne conserver que le produit 4
sales = sales[sales["product"] == 4]

# Une colonne par magasin
sales_by_store = sales.pivot_table(
    index="Date",
    columns="store",
    values="number_sold",
    aggfunc="sum"
)

# Agrégation mensuelle
sales_by_store = sales_by_store.resample("ME").sum()

print(sales_by_store.head())
```

On obtient un tableau dont chaque colonne représente une série temporelle distincte.

`pivot_table()` permet de passer du format long (une ligne par vente/magasin/date) au format large (une colonne par magasin).

## 3. Visualiser les séries

```python
plt.figure(figsize=(12, 6))
for store in sales_by_store.columns:
    plt.plot(sales_by_store.index, sales_by_store[store], label=f'Store {store}')
plt.title('Sales by Store')
plt.xlabel('Date')
plt.ylabel('Sales')
plt.legend()
plt.show()
```
![[Pasted image 20261020085951.png]]
Observer :

- Les tendances communes ou différentes.
    
- La présence de saisonnalités.
    
- Les écarts entre magasins.
    

La visualisation permet de repérer d'éventuelles relations, mais ne démontre pas une dépendance entre les séries.

## 4. Séparer chronologiquement train et test

```python
train = sales_by_store.iloc[:-12].copy()
test = sales_by_store.iloc[-12:].copy()
```

Les 12 derniers mois servent à évaluer les prévisions.

**Ne pas effectuer de séparation aléatoire** : les observations futures ne doivent pas servir à entraîner le modèle.

Dans l'étude de cas Liora, les ventes de 2019 sont fournies dans un fichier séparé. On utilise donc les données antérieures pour l'entraînement et `sales2019.csv` pour l'évaluation.

## 5. Examiner la stationnarité

```python
for magasin in train.columns:
    resultat = adfuller(train[magasin].dropna())

    print(
        f"Magasin {magasin} : "
        f"p-value = {resultat[1]:.4f}"
    )
```

Le test ADF examine l'hypothèse de racine unitaire.

- `p-value < 0.05` : on rejette l'hypothèse de racine unitaire.
    
- `p-value >= 0.05` : on ne peut pas rejeter cette hypothèse.
    

Le test est un indicateur à interpréter avec les graphiques. Pour utiliser un VAR standard, il faut notamment s'assurer que les séries présentent des propriétés compatibles avec la stationnarité.

Si une différenciation est nécessaire :

```python
train_diff = train.diff().dropna()
```

Cette transformation doit être prise en compte lors de la reconstruction des prévisions sur l'échelle originale.

## 6. Instancier un modèle VAR

```python
model = VAR(train)
```

`VAR` reçoit un `DataFrame` contenant toutes les séries à modéliser.

Contrairement à `ARIMA`, le modèle utilise plusieurs variables endogènes simultanément.

Dans cet exemple, `train` doit être stationnaire avant l'ajustement. Si ce n'est pas le cas, utiliser une préparation adaptée plutôt que poursuivre automatiquement sur les niveaux bruts.

## 7. Ajuster le modèle VAR

```python
# Exemple du cours : ordre p = 3
results = model.fit(3)

print(results.summary())
```

**VAR(3)** signifie que chaque magasin est prédit à partir :

- De ses propres ventes aux trois périodes précédentes.
    
- Des ventes des autres magasins aux trois périodes précédentes.
    

Avec sept magasins et trois retards, chaque équation comporte **21 coefficients de retard**, plus une constante si elle est incluse.

`summary()` affiche les coefficients, leurs statistiques et les relations résiduelles entre les séries.

## 8. Comparer les ordres du modèle

```python
bic_scores = {}

for p in range(1, 6):
    model=VAR(sales_by_store)
    results_p = model.fit(p)
    bic_scores[p] = results_p.bic

scores = pd.Series(bic_scores)

print(scores)
print("Meilleur ordre :", scores.idxmin())
```

Le **BIC** mesure un compromis entre qualité d'ajustement et complexité.

**Plus le BIC est faible, meilleur est le compromis selon ce critère.**

Dans l'exercice Liora, la comparaison des ordres de 1 à 5 aboutit à **p = 2**. Ce résultat concerne ce jeu de données et ne doit pas être généralisé.

## 9. Entraîner le modèle retenu

```python
best_p = scores.idxmin()

results_final = model.fit(best_p)

print(results_final.summary())
```

Le modèle retenu utilise les `best_p` périodes précédentes de chaque série.

---

# Évaluer le modèle

## 10. Produire les prévisions

```python
forecast = results_final.forecast(
    y=train.values[-results_final.k_ar:],
    steps=len(test)
)

forecast_df = pd.DataFrame(
    forecast,
    index=test.index,
    columns=train.columns
)


#exemple de cours
model = VAR(sales_by_store)
results = model.fit(2)

forecast = results.forecast(sales_by_store.values, steps=12)
forecast_df = pd.DataFrame(forecast)

forecast_df.index = pd.date_range('01-01-2019', periods=12, freq='ME')
forecast_df
```

- `y` : les dernières observations nécessaires au modèle.
    
- `steps` : nombre de périodes à prédire.
    
- `forecast_df` : prévisions organisées avec les mêmes dates et magasins que le test.
    

Ces prévisions sont directement comparables au test **si le modèle a été entraîné sur l'échelle originale stationnaire**. Après différenciation, une reconstruction est nécessaire.

## 11. Calculer les métriques

```python
mae = mean_absolute_error(test, forecast_df)

mape = mean_absolute_percentage_error(test, forecast_df)

r2 = r2_score(test, forecast_df)

print("MAE :", mae)
print("MAPE :", mape * 100, "%")
print("R² :", r2)
```

- **MAE** : erreur absolue moyenne, exprimée en unités vendues.
    
- **MAPE** : erreur relative moyenne en pourcentage.
    
- **R²** : qualité globale des prédictions, selon la méthode d'agrégation choisie.
    

Avec plusieurs magasins, on peut également calculer les scores **séparément pour chaque magasin**.

## 12. Comparer visuellement réel et prédit

```python
magasin = test.columns[0]

plt.figure(figsize=(10, 4))

plt.plot(
    test.index,
    test[magasin],
    label="Réel"
)

plt.plot(
    forecast_df.index,
    forecast_df[magasin],
    label="Prédit",
    linestyle="--"
)

plt.title(f"Prévisions du magasin {magasin}")
plt.legend()
plt.show()
```

Rechercher :

- Un biais de surestimation ou de sous-estimation.
    
- Des pics saisonniers mal anticipés.
    
- Une dégradation des prévisions au fil des mois.
    

---

# Extensions — VARMA et VARIMA

## 13. VARMA : ajouter les innovations passées

Le modèle **VARMA(p,q)** combine :

- **VAR(p)** : influence des valeurs passées de toutes les séries.
    
- **VMA(q)** : influence des innovations passées, y compris les relations entre séries.
    

Il prolonge le principe de [[AR → MA → ARMA → ARIMA → SARIMA]] dans un contexte multivarié.

Dans `statsmodels`, la classe `VARMAX` permet d'estimer notamment un modèle VARMA :

```python 
from statsmodels.tsa.statespace.varmax import VARMAX

model_varma = VARMAX(
    train,
    order=(2, 1)
)

results_varma = model_varma.fit(
    disp=False
)

print(results_varma.summary())
```

**Attention :** ce code est une illustration hors du workflow pratique Liora. Les modèles VARMA peuvent être difficiles à estimer, notamment à cause de problèmes d'identifiabilité et de convergence. Ce n'est pas nécessairement une amélioration automatique du VAR.

## 14. VARIMA : intégrer la différenciation

Le modèle **VARIMA(p,d,q)** étend VARMA en introduisant une différenciation, comme ARIMA le fait pour ARMA.

Le principe est :

1. Transformer les séries pour traiter une non-stationnarité.
    
2. Modéliser leurs dépendances multivariées avec des composantes AR et MA.
    
3. Reconstruire les prévisions sur l'échelle initiale.
    

Dans le cadre présenté par le cours Liora, **aucun workflow VARIMA complet n'est fourni**.

`statsmodels` ne propose pas une interface simple `VARIMA(order=(p,d,q))` équivalente à celle d'ARIMA pour toutes les séries simultanément.

On peut, dans certains cas, ajuster un modèle multivarié sur des séries préalablement différenciées, mais cela exige de gérer correctement les transformations et leur inversion.

**Ne pas assimiler automatiquement** `**VAR(train.diff())**` **à un workflow VARIMA complet :** ce serait seulement un VAR estimé sur des différences, sans composante MA.

---

# Pattern minimal

Pour plusieurs séries mensuelles déjà préparées, stationnaires et sans valeurs manquantes :

```python
from statsmodels.tsa.api import VAR

# Séparation chronologique
train = data.iloc[:-12]
test = data.iloc[-12:]

# Modèle VAR
model = VAR(train)

# Sélection de l'ordre
selection = model.select_order(maxlags=5)
p = selection.selected_orders["bic"]

# Ajustement
results = model.fit(p)

# Prévisions
forecast = results.forecast(
    train.values[-results.k_ar:],
    steps=len(test)
)

# Mise en DataFrame
predictions = pd.DataFrame(
    forecast,
    index=test.index,
    columns=train.columns
)
```

---

## Points de vigilance

- **VAR n'est pas SARIMA multivarié** : le VAR standard n'intègre pas directement de structure saisonnière explicite.
    
- Les séries doivent être **alignées sur les mêmes dates**, avec une fréquence régulière.
    
- VAR standard et VARMA stationnaire nécessitent un examen de la stationnarité avant ajustement.
    
- Plus il y a de magasins et de retards, plus le nombre de coefficients augmente.
    
- Un ordre `p` élevé ne garantit pas de meilleures prévisions.
    
- Comparer les ordres sur le train, puis conserver une période future pour l'évaluation.
    
- Après différenciation ou transformation logarithmique, **revenir à l'échelle originale** avant de calculer les erreurs métier.
    
- Les métriques globales peuvent masquer des performances très différentes selon les magasins.
    
- VARMA et VARIMA sont des extensions plus complexes ; dans le notebook Liora, seul **VAR** fait l'objet d'un entraînement détaillé.
    

## Références

- Support Liora — _Time series : Analyses et prédictions_, étude de cas, partie **III. Séries Multi-Variées**.
   