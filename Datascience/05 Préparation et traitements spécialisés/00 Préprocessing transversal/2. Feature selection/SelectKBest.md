# `SelectKBest`

## Syntaxe

```python
from sklearn.feature_selection import SelectKBest, f_classif

selecteur = SelectKBest(score_func=f_classif, k=5)

X_train_selectionne = selecteur.fit_transform(X_train, y_train)
X_test_selectionne = selecteur.transform(X_test)
```

Pour examiner les scores avant de fixer `k` :

```python
selecteur = SelectKBest(score_func=f_classif, k="all")
selecteur.fit(X_train, y_train)

scores = selecteur.scores_
```

## Paramètres

- `score_func` : fonction utilisée pour mesurer séparément la relation entre chaque variable explicative et la cible.
- `k=5` : conserve les 5 variables ayant les meilleurs scores.
- `k="all"` : ne supprime aucune variable ; utile pour examiner les scores avant de choisir `k`.
- `X_train` : variables explicatives d'entraînement.
- `y_train` : cible d'entraînement utilisée pour évaluer chaque variable.

### Choisir `score_func`

| Cible `y`    | Variables `X`                     | `score_func`             | À choisir quand…                                                    |
| ------------ | --------------------------------- | ------------------------ | ------------------------------------------------------------------- |
| quantitative | numériques                        | `f_regression`           | on cherche surtout une relation linéaire avec la cible              |
| quantitative | numériques                        | `mutual_info_regression` | la relation peut être non linéaire / plus générale                  |
| catégorielle | numériques                        | `f_classif`              | on cherche si les valeurs numériques diffèrent selon les classes    |
| catégorielle | numériques                        | `mutual_info_classif`    | la relation avec les classes peut être non linéaire / plus générale |
| catégorielle | comptes / fréquences non négatifs | `chi2`                   | les features sont ≥ 0 et adaptées au χ²                             |

Repère rapide :

```text
y quantitative
→ régression
→ f_regression / mutual_info_regression

y catégorielle
→ classification
→ f_classif / mutual_info_classif

y catégorielle + X non négatives de type comptes/fréquences
→ chi2
```

## Fonction

`SelectKBest` réalise une **sélection supervisée de variables** : chaque feature est évaluée individuellement par rapport à `y`, puis seules les `k` mieux classées sont conservées.

## Vigilances & tips

Le choix de `score_func` dépend **à la fois de la nature de la cible et des variables explicatives**. Une cible catégorielle ne signifie donc pas automatiquement `chi2`.

`chi2` exige des valeurs de `X` **non négatives**. Il ne convient pas, par exemple, à des variables standardisées pouvant devenir négatives.

`SelectKBest` travaille sur des features numériques. Les variables catégorielles doivent être encodées avant la sélection.

La sélection apprend à partir de `y` : pour éviter la fuite de données, faire `fit` / `fit_transform` uniquement sur le **train**, puis seulement `transform` sur le test.

En pratique, `SelectKBest` est souvent intégré dans un `Pipeline` afin que la sélection soit correctement réapprise à l'intérieur de chaque fold de validation croisée.

`SelectKBest` évalue les variables **une par une** par rapport à la cible : il ne détecte pas nécessairement qu'une variable peu informative seule devient utile en interaction avec d'autres.

**S'utilise souvent avec :** `Pipeline`, `GridSearchCV`, `get_support()`

# Association

```python
selecteur.get_feature_names_out #affiche le nom des variables conservées de la classe instanciée
```