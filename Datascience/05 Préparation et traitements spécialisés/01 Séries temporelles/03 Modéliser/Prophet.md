## Quand l’utiliser ?

Préparer une **série temporelle datée**, ajuster un modèle **Prophet**, visualiser la prévision et ses composantes, puis évaluer les prédictions sur une période future conservée pour le test.

**Point de départ du cours Liora :** `AirPassengers.csv`, observations **mensuelles** du nombre de passagers. Contrairement au workflow ARIMA utilisant `airpasslog`, on part ici de la **série originale**. Le code suivant reprend les étapes du notebook Prophet ; la section d'optimisation est facultative.

## Principe — repères rapides

|Besoin|Commande|
|---|---|
|Format exigé|DataFrame avec `ds` (date) et `y` (valeur numérique)|
|Initialiser|`Prophet(...)`|
|Entraîner|`model.fit(df_train)`|
|Générer les dates futures|`model.make_future_dataframe(periods=24, freq="MS")`|
|Prévoir|`model.predict(future)`|
|Afficher la prévision|`model.plot(forecast)`|
|Afficher les composantes|`model.plot_components(forecast)`|
|Récupérer les résultats|`forecast[["ds", "yhat", "yhat_lower", "yhat_upper"]]`|

**Différence avec ARIMA :** Prophet ne demande pas de différencier manuellement la série pour la rendre stationnaire avant l'ajustement.

---

# Pas à pas

## 1. Importer les bibliothèques et les données

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from prophet import Prophet
from sklearn.metrics import mean_squared_error, mean_absolute_error

df = pd.read_csv("AirPassengers.csv", parse_dates=["Month"])
print(df.head())
print(df.dtypes)
```

`parse_dates=["Month"]` convertit la colonne de dates. Le fichier doit contenir les colonnes `Month` et `#Passengers` (noms du jeu AirPassengers couramment utilisé dans ce cours).

## 2. Mettre les colonnes au format Prophet

```python
df = df.rename(columns={"Month": "ds", "#Passengers": "y"})
df = df[["ds", "y"]].sort_values("ds").reset_index(drop=True)

df["ds"] = pd.to_datetime(df["ds"])
df["y"] = pd.to_numeric(df["y"], errors="coerce")

print(df.head())
print(df.isna().sum())
print(df["ds"].duplicated().sum())
```

- `ds` : dates de la série.
- `y` : valeur à prédire.
- Vérifier les dates dupliquées ou les valeurs manquantes **avant** de modéliser ; ne pas supprimer des lignes ou inventer des dates sans examiner le problème.

## 3. Visualiser la série

```python
plt.figure(figsize=(10, 4))
plt.plot(df["ds"], df["y"])
plt.xlabel("Date")
plt.ylabel("Passagers")
plt.title("AirPassengers — série mensuelle")
plt.show()
```

Observer la tendance et les variations saisonnières. Cette visualisation ne constitue pas encore une prévision.

## 4. Découper le train et le test chronologiquement

```python
# Les 24 derniers mois sont réservés au test (consigne Liora)
df_train = df.iloc[:-24]
df_test = df.iloc[-24:]

print("Train :", df_train["ds"].min(), "→", df_train["ds"].max())
print("Test  :", df_test["ds"].min(), "→", df_test["ds"].max())
```

Pas de `train_test_split` aléatoire : on entraîne sur le passé et on teste sur les **24 mois suivants**.

## 5. Instancier et entraîner Prophet

```python
model = Prophet()
model.fit(df_train)
```

Le modèle apprend une tendance et des effets saisonniers à partir du train. Ici, on commence avec les paramètres par défaut, comme dans la première partie du cours.

## 6. Créer les dates à prédire

```python
future = model.make_future_dataframe(periods=24, freq="MS")
print(future.tail())
```

- `periods=24` : prolonger de 24 observations mensuelles.
- `freq="MS" : dates en **début de mois** (_Month Start_), adaptées aux dates usuelles d'AirPassengers.
- Par défaut, `future` contient aussi les dates du train.

**Vérification :** les dates des 24 dernières lignes de `future` doivent correspondre à `df_test["ds"]`. Si les observations sont en **fin de mois**, remplacer la fréquence par `"ME"`. Ne pas aligner artificiellement des dates incorrectes.

## 7. Produire les prévisions

```python 
forecast = model.predict(future)

print(forecast[["ds", "yhat", "yhat_lower", "yhat_upper"]].tail())
```

- `yhat` : valeur prédite.
- `yhat_lower` et `yhat_upper` : bornes de l'intervalle d'incertitude de prévision fourni par Prophet.

## 8. Visualiser la prévision et les composantes

```python
model.plot(forecast)
plt.show()

model.plot_components(forecast)
plt.show()
```

Le premier graphique affiche la prévision et son intervalle ; le second détaille les composantes estimées (notamment tendance et saisonnalité annuelle si elle est activée).

## 9. Isoler les prévisions correspondant au test

```python
pred_test = df_test[["ds", "y"]].merge(
    forecast[["ds", "yhat", "yhat_lower", "yhat_upper"]],
    on="ds",
    how="left",
    validate="one_to_one"
)

print(pred_test.head())
print("Prévisions manquantes :", pred_test["yhat"].isna().sum())
```

La jointure sur `ds` garantit qu'on compare **les mêmes dates**. Si des prévisions manquent, vérifier `freq`, les dates du jeu source et l'horizon avant de calculer des scores.

---

# Évaluer le modèle

## 10. Calculer MAE et RMSE sur le test

```python
# À exécuter uniquement si aucune prévision n'est manquante
mae = mean_absolute_error(pred_test["y"], pred_test["yhat"])
rmse = np.sqrt(mean_squared_error(pred_test["y"], pred_test["yhat"]))

print("MAE :", mae)
print("RMSE :", rmse)


from sklearn.metrics import root_mean_squared_error

rmse = root_mean_squared_error(df_test['y'], forecast['yhat'].tail(24))
print(f'Erreur quadratique moyenne (RMSE) : {rmse}')
```

Les erreurs sont exprimées ici en **nombre de passagers**, puisque la cible `y` n'a pas été transformée en logarithme.

## 11. Comparer visuellement le réel et le prédit

```python
plt.figure(figsize=(10, 6))
plt.plot(df_test['ds'], df['y'].tail(24), label='Valeurs réelles')
plt.plot(df_test['ds'], forecast['yhat'].tail(24), label='Prédictions', linestyle='--')
plt.fill_between(df_test['ds'], forecast['yhat_lower'].tail(24), forecast['yhat_upper'].tail(24), 
                 color='orange', alpha=0.15, label='Intervalle de confiance (80%)')
plt.title('Prédictions de Prophet')
plt.xlabel('Date')
plt.xticks(df_test['ds'], rotation=45)
plt.ylabel('Nombre de passagers')
plt.legend()
plt.show()
```

Chercher une sous-estimation ou surestimation systématique, un décalage des pics saisonniers et une dégradation en fin d'horizon.
![[Pasted image 20261019111208.png]]
## 12. Examiner les erreurs de prévision

```python
pred_test = modele[["ds", "y"]].merge(
    forecast[["ds", "yhat"]],
    on="ds",
    how="left"
)

pred_test["residu"] = pred_test["y"] - pred_test["yhat"]

plt.figure(figsize=(10, 4))
plt.plot(pred_test["ds"], pred_test["residu"])
plt.axhline(0, color="black", linestyle="--")
plt.title("Erreurs de prévision : réel - prédit")
plt.show()
```

**Attention :** ces erreurs sur le **test** sont différentes des résidus d'une décomposition descriptive comme `seasonal_decompose()`.

---

# Optimisation (facultatif — après le premier modèle)

Cette partie reprend la progression du notebook Liora, sans remplacer l'évaluation finale sur le test.
Dans la documentation de Prophet, il est suggéré d'optimiser 5 hyperparamètres :

- **changepoint_prior_scale** : Il détermine la flexibilité de la tendance, et en particulier à quel point la tendance change aux points de changement de tendance. S'il est trop faible, la tendance sera sous-ajustée et la variance qui aurait dû être modélisée par des changements de tendance sera plutôt traitée par le terme de bruit. S'il est trop élevé, la tendance sera sur-ajustée et dans le cas le plus extrême, vous pourriez vous retrouver avec une tendance égale à la saisonnalité annuelle. Une plage de variation de [0.001, 0.5] est conseillée, avec une valeur par défaut de 0.05.
    
- **seasonality_prior_scale** : Ce paramètre contrôle la flexibilité de la saisonnalité. De manière similaire, une grande valeur permet à la saisonnalité de s'ajuster à de grandes fluctuations, tandis qu'une petite valeur réduit l'ampleur de la saisonnalité. La valeur par défaut est 10, et une plage raisonnable pour l'ajuster est [0.01, 10].
    
- **holidays_prior_scale** : Cela contrôle la flexibilité pour ajuster les effets des jours fériés. Il a la même valeur par défaut et la même plage conseillée que **seasonality_prior_scale**.
    
- **seasonality_mode** : Il définit si nous avons un modèle additif ou multiplicatif.
    
- **changepoint_range** : C'est la proportion des données d'entrainement dans laquelle la tendance est autorisée à changer. Cela est par défaut à 0.8, ce qui signifie que le modèle ne s'ajustera pas aux changements de tendance dans les derniers 20% de la série temporelle. Cela permet d'éviter le surajustement aux changements de tendance à la toute fin de la série temporelle où il n'y a pas suffisamment de marge pour bien l'ajuster.
## 13. Fixer les options saisonnières de la série mensuelle

```python
param_fixed = {
    "seasonality_mode": "multiplicative",
    "daily_seasonality": False,
    "weekly_seasonality": False,
    "yearly_seasonality": True
}
```

Le cours retient ici une saisonnalité **multiplicative** pour AirPassengers. Il n'utilise pas d'effet de jours fériés.

## 14. Définir une grille réduite pour les réglages

```python
param_grid = {
    "changepoint_prior_scale": [0.01, 0.1],
    "seasonality_prior_scale": [0.1, 1.0],
    "changepoint_range": [0.8, 0.9]
}
```

Grille **illustrative et réduite** pour limiter les calculs : ce ne sont pas des valeurs optimales garanties. Les trois paramètres correspondent à ceux étudiés dans le cours.

## 15. Valider chronologiquement sur le train

```python
import itertools
from prophet.diagnostics import cross_validation, performance_metrics

all_params = [
    dict(zip(param_grid.keys(), valeurs))
    for valeurs in itertools.product(*param_grid.values())
]

resultats = []

for params in all_params:
    modele_cv = Prophet(**param_fixed, **params)
    modele_cv.fit(df_train)

    cv = cross_validation(
        modele_cv,
        initial="2920 days",
        period="365 days",
        horizon="365 days"
    )
    perf = performance_metrics(cv, rolling_window=1)

    rmses.append({**params, "rmse": perf["rmse"].mean()})

classement = pd.DataFrame(all_params)
classement["RMSE"] = rmses

classement = classement.sort_values(by="RMSE")
display(classement)

best_params = all_params[np.argmin('rmses')]
best_score = tuning_results['rmse'].min()
print(best_params)
print(best_score)
```

Ces fenêtres sont celles présentées dans le cours. **À adapter à la longueur de la série et à l'horizon réellement visé** : la validation croisée doit rester entièrement dans `df_train`. Cette étape peut être coûteuse ; elle n'est pas nécessaire pour faire fonctionner le premier workflow.

## 16. Réentraîner le meilleur candidat et tester une seule fois

```python
meilleurs_params = classement.iloc[0][list(param_grid)].to_dict()

model_tuned = Prophet(**best_params, **param_fixed)
model_tuned.fit(df_train)
predictions_tuned = model_tuned.predict(future)

# Ici on passe les dates EXACTES du jeu test.
forecast_opt = model_opt.predict(df_test[["ds"]])

rmse_opt = np.sqrt(mean_squared_error(df_test["y"], forecast_opt["yhat"]))
print("RMSE test (modèle optimisé) :", rmse_opt)
```

Le jeu de test doit rester hors du choix des hyperparamètres. Comparer ce RMSE avec le modèle initial.

---

# Pattern minimal

Pour une série mensuelle déjà préparée au format `ds` / `y` :

```python
from prophet import Prophet

train = df.iloc[:-24].copy()
test = df.iloc[-24:].copy()

model = Prophet()
model.fit(train)

# Les dates du test sont connues : plus sûr pour l'évaluation
forecast = model.predict(test[["ds"]])

resultats = test[["ds", "y"]].merge(
    forecast[["ds", "yhat", "yhat_lower", "yhat_upper"]],
    on="ds"
)
```

## Points de vigilance

- **Prophet exige les colonnes `ds` et `y`** ; `ds` contient de vraies dates et `y` une cible numérique.
- **Conserver la chronologie** et isoler les 24 derniers mois du test avant l'entraînement ou l'optimisation.
- `freq="MS"` désigne le **début de mois** ; vérifier la convention des dates avant d'utiliser `make_future_dataframe()`.
- `periods=24` indique **24 périodes**, pas 24 mois si la fréquence est journalière par défaut : renseigner `freq`.
- Les intervalles `yhat_lower` / `yhat_upper` représentent une **incertitude de prévision**, pas une garantie de couverture.
- Le `seasonality_mode="multiplicative"` est un choix du **cas AirPassengers**, pas un réglage universel.
- La CV de Prophet utilise des **fenêtres temporelles** ; ne pas optimiser sur le test final.
- Ne pas confondre les erreurs du test avec les composantes résiduelles d'une décomposition.

## Références

- Support Liora fourni : `SEries temporelles(1).md`, partie **Prophet** (importation, préparation, modélisation, visualisation, RMSE, optimisation).
- [Prophet — Quick Start](https://facebook.github.io/prophet/docs/quick_start.html)
- [Prophet — Diagnostics](https://facebook.github.io/prophet/docs/diagnostics.html)
- [Prophet — Seasonality](https://facebook.github.io/prophet/docs/seasonality,_holiday_effects,_and_regressors.html)

