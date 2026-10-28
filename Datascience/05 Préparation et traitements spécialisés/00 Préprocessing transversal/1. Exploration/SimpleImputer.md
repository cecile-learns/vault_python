

# SimpleImputer

## Quand l’utiliser ?

Remplacer automatiquement les valeurs manquantes d’un jeu de données avant l’entraînement d’un modèle.

Dans le cours :

- variables numériques → remplacement par la **médiane** ;
- variables catégorielles → remplacement par la **modalité la plus fréquente**.

L’imputation doit être réalisée **après le `train_test_split`** afin d’éviter une fuite de données.

# Principe

`SimpleImputer` suit le même principe général que `StandardScaler` :

```text
fit
→ apprend une valeur à partir du TRAIN

transform
→ applique cette valeur aux données
````

Exemple avec la médiane :

```
X_train
   ↓
fit
   ↓
calcule et mémorise
la médiane de chaque colonne
   ↓
transform(X_train)
   ↓
remplace les NaN du train

transform(X_test)
   ↓
réutilise les médianes
apprises sur le train
```

Le test **ne doit pas servir à calculer ses propres valeurs d’imputation**.

# Identifier les colonnes

Dans le cours :

```
numerical_columns = [    'satisfaction_level',    'last_evaluation',    'average_monthly_hours']categorical_columns = [    'number_projects',    'time_spend_company',    'work_accident',    'promotion_last_5_years',    'position',    'salary']
```

Attention : une variable peut contenir des nombres tout en étant considérée comme **catégorielle**.

Exemple :

```
work_accident
0 = non
1 = oui
```

Le type informatique seul ne suffit donc pas toujours à déterminer la stratégie d’imputation.

# Imputer les variables numériques

Créer un imputateur qui utilise la médiane :

```
from sklearn.impute import SimpleImputernumerical_imputer = SimpleImputer(strategy='median')
```

Puis :

```
X_train[numerical_columns] = numerical_imputer.fit_transform(    X_train[numerical_columns])X_test[numerical_columns] = numerical_imputer.transform(    X_test[numerical_columns])
```

**Ce qui se passe**

```
fit_transform(X_train)
→ calcule les médianes du train
→ les mémorise
→ remplace les NaN du train

transform(X_test)
→ ne recalcule rien
→ utilise les médianes du train
→ remplace les NaN du test
```

# Imputer les variables catégorielles

Créer un second imputateur :

```python
categorical_imputer = SimpleImputer(strategy='most_frequent')
```

Puis :

```python
X_train[categorical_columns] = categorical_imputer.fit_transform(
                               X_train[categorical_columns])

X_test[categorical_columns] = categorical_imputer.transform(
                              X_test[categorical_columns])
```

`most_frequent` correspond à la **modalité la plus fréquente** de chaque colonne.

---

# Pattern complet

```Python
from sklearn.impute import 
SimpleImputernumerical_columns = ['satisfaction_level','last_evaluation',    'average_monthly_hours']
categorical_columns = ['number_projects','time_spend_company','work_accident',    'promotion_last_5_years','position','salary']

# Numérique → médiane
numerical_imputer = SimpleImputer(strategy='median')

X_train[numerical_columns] = numerical_imputer.fit_transform(
                             X_train[numerical_columns])

X_test[numerical_columns] = numerical_imputer.transform(
                             X_test[numerical_columns])

# Catégoriel → modalité la plus fréquente
categorical_imputer = SimpleImputer(strategy='most_frequent')

X_train[categorical_columns] = categorical_imputer.fit_transform(    
                        X_train[categorical_columns])
                        
X_test[categorical_columns] = categorical_imputer.transform(
                              X_test[categorical_columns])
```

---

# Paramètre utile

## `strategy`

Détermine comment calculer la valeur utilisée pour remplacer les données manquantes.

Dans le cours :

```
strategy='median'
```

→ médiane de chaque colonne.

```
strategy='most_frequent'
```

→ valeur la plus fréquente de chaque colonne.

---

# Équivalent avec Pandas

Pour comprendre ce que fait `SimpleImputer`, l'équivalent manuel est proche de :

```python
median = X_train['age'].median()X_train['age'] = X_train['age'].fillna(median)X_test['age'] = X_test['age'].fillna(median)
```

Pour une variable catégorielle :

```python
mode = X_train['position'].mode()[0]X_train['position'] = X_train['position'].fillna(mode)X_test['position'] = X_test['position'].fillna(mode)
```

`SimpleImputer` encapsule cette logique dans un objet sklearn capable de **mémoriser les valeurs apprises pendant `fit()`**.

---

# Pattern minimal

Avec les colonnes déjà sélectionnées :

```Python
imputer = SimpleImputer(strategy='median')
X_train_num = imputer.fit_transform(X_train[num_cols])
X_test_num = imputer.transform(X_test[num_cols])
```

Pattern fondamental :

```
TRAIN → fit_transform()
TEST  → transform()
```

---

# Points de vigilance

- Faire le `train_test_split` **avant** l’imputation lorsque la valeur de remplacement dépend des données.
- Ne jamais faire :

```python
imputer.fit_transform(X_test)
```

car le test apprendrait alors ses propres paramètres.

- Les valeurs utilisées pour `X_test` doivent provenir de **`X_train`**.
- Ne pas décider qu’une variable est numérique uniquement parce qu’elle contient des nombres.
- Utiliser des stratégies différentes pour les groupes de variables lorsque leur nature le nécessite.
- `SimpleImputer` remplace les valeurs manquantes : il ne décide pas quelles variables sont numériques ou catégorielles. Cette sélection doit être faite avant.

---

# À retenir

```
SimpleImputer
     ↓
apprend une valeur de remplacement
sur le TRAIN
     ↓
mémorise cette valeur
     ↓
l'applique au TRAIN et au TEST
```

Même logique sklearn que :

```
StandardScaler
OneHotEncoder
SimpleImputer

fit sur train
→ transform sur train
→ transform sur test
```

## Références

Source de formation : `Masterclass ML.md`.

Cours utilisé :

- imputation après le split pour éviter la fuite de données ;
- médiane pour les variables numériques ;
- modalité la plus fréquente pour les variables catégorielles ;
- `fit_transform()` sur le train puis `transform()` sur le test. Masterclass ML

Documentation sklearn : `sklearn.impute.SimpleImputer`
