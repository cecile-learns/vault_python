
# LazyPredict — Comparer rapidement des modèles ML

## Quand l'utiliser ?

Au début de la modélisation supervisée, pour comparer rapidement plusieurs algorithmes et identifier des modèles candidats sans régler individuellement leurs hyperparamètres.

**Principe** LazyPredict entraîne plusieurs modèles, évalue leurs performances et produit un tableau comparatif. Il facilite la présélection, mais ne remplace ni l'optimisation ni l'évaluation finale.

## 1. Préparer les données

**Ce que je fais** Je définis `X` et `y`, isole le jeu de test final, puis sépare le train en apprentissage et validation pour le benchmark.

**Vigilance** Le test final ne doit pas servir à choisir le modèle. Les exemples officiels utilisent souvent directement train/test : pour un véritable projet ML, privilégier train/validation à ce stade.

## 2. Choisir la classe selon la cible

|Situation|Classe|Indicateurs principaux|
|---|---|---|
|Classification|`LazyClassifier`|Accuracy, Balanced Accuracy, F1, ROC-AUC|
|Régression|`LazyRegressor`|R², RMSE, R² ajusté|
|Séries temporelles|`LazyForecaster`|MAE, RMSE et métriques de prévision|

## 3. Exécuter le benchmark

**Installation**

```python
pip install lazypredict
```

**Classification**

```python
from lazypredict.Supervised import LazyClassifier

clf = LazyClassifier(
    verbose=0,
    ignore_warnings=False
)

resultats, predictions = clf.fit(
    X_train, X_val, y_train, y_val
)

display(resultats)
```

**Régression — alternative**

```python
from lazypredict.Supervised import LazyRegressor

reg = LazyRegressor(
    verbose=0,
    ignore_warnings=False
)

resultats, predictions = reg.fit(
    X_train, X_val, y_train, y_val
)

display(resultats)
```

`X_val` et `y_val` représentent ici le jeu de validation, pas le test final.

**Sortie attendue** Un DataFrame comparant les modèles et leurs performances. Le retour des prédictions individuelles dépend de l'option `predictions`.

## 4. Lire et trier les résultats

![[Pasted image 20261008120108.png]]

|Indicateur|Interprétation|
|---|---|
|Accuracy|Proportion totale de prédictions correctes|
|Balanced Accuracy|Moyenne des rappels par classe, utile en cas de déséquilibre|
|F1 Score|Compromis précision/rappel ; vérifier la convention de moyenne utilisée|
|ROC AUC|Discrimination des classes ; vérifier son mode de calcul|
|R-Squared|Part de variance expliquée par le modèle|
|RMSE|Erreur de prédiction exprimée dans l'unité de la cible|
|Time Taken|Temps de calcul observé pour le modèle|

**Ce que je fais** Je trie selon la métrique métier pertinente, puis retiens quelques modèles candidats présentant un compromis acceptable entre performance, temps de calcul et interprétabilité.

Exemple :

```python
resultats.sort_values(
    "Balanced Accuracy",
    ascending=False
).head(5)
```

## 5. Après LazyPredict : reprendre la main

**Ce que je fais**

1. Sélectionner 2 à 4 modèles candidats.
    
2. Construire pour chacun un pipeline maîtrisé.
    
3. Comparer par validation croisée avec une métrique commune.
    
4. Optimiser si nécessaire avec `GridSearchCV`, `RandomizedSearchCV` ou une approche bayésienne.
    
5. Évaluer le modèle final sur le test resté à l'écart.
    

**Sortie attendue** Un modèle sélectionné pour des raisons explicites, et non simplement parce qu'il apparaît en première ligne.

## Paramètres utiles

|Paramètre|Utilité|
|---|---|
|`verbose`|Contrôler l'affichage|
|`ignore_warnings`|Masquer ou afficher les avertissements|
|`custom_metric`|Ajouter une métrique métier personnalisée|
|`categorical_encoder`|Choisir `onehot`, `ordinal`, `target` ou `binary`|
|`classifiers` / `regressors`|Limiter les algorithmes testés|
|`timeout`|Limiter le temps par modèle|
|`cv`|Activer une validation croisée intégrée|
|`predictions=True`|Demander les prédictions des modèles|

Options avancées : vérifier la compatibilité avec la version installée.

## Points de vigilance

- **Pas d'optimisation systématique des hyperparamètres** : les scores sont avant tout exploratoires.
    
- **Préprocessing automatique** : pratique pour comparer, mais ne dispense pas d'examiner les transformations appliquées.
    
- **Fuite de données** : ne pas utiliser le test final pour classer puis sélectionner les modèles.
    
- **Encodage supervisé** : vérifier particulièrement l'absence de fuite dans le calcul des statistiques liées à `y`.
    
- **Classes déséquilibrées** : ne pas se fier uniquement à l'accuracy ou au F1 global.
    
- **Séries temporelles** : respecter l'ordre chronologique et utiliser une validation temporelle.
    
- **Modèles en échec** : avec `ignore_warnings=True`, des erreurs peuvent être masquées.
    
- **Reproductibilité** : fixer les graines aléatoires lorsque les estimateurs et la configuration le permettent.
    

## À retenir

**LazyPredict = outil de benchmark, pas outil de sélection finale automatique.**

Données préparées → train/validation → LazyPredict → modèles candidats → validation croisée / optimisation → test final.

## Références

- [GitHub — LazyPredict](https://github.com/shankarpandala/lazypredict)
    
- [PyPI — lazypredict](https://pypi.org/project/lazypredict/)
    
- [Documentation — exemples](https://lazypredict.readthedocs.io/en/latest/examples.html)
    

Version de référence : `0.3.0` (mars 2026). Certaines installations de formation peuvent utiliser une version antérieure.