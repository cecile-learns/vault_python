
## Quand l’utiliser ?

Pendant l’EDA et la préparation des données pour :

- automatiser l’analyse répétitive des variables ;
- séparer `X` et `y` proprement ;
- regrouper dans une fonction les transformations nécessaires pour reproduire un preprocessing.

> Ces fonctions sont des **patterns à adapter au dataset**, pas des recettes universelles.

---

# 1. `analyse_variable_binaire()`

## Quand l’utiliser ?

Pour explorer rapidement une **variable catégorielle binaire** en regardant :

- sa répartition ;
- sa relation avec la cible ;
- sa distribution graphique.

Le cours l’utilise notamment pour `Driving_License`, `Previously_Insured` et `Vehicle_Damage`. Texte collé

## Pattern du cours

```python
def analyse_variable_binaire(variable, base):    
    print("Répartition de la variable: ", end="\n\n")    
    print(   
           base[variable].value_counts(            
           normalize=True,            
           dropna=False        
           ),        
           end="\n\n"    
           )    
    print("Matrice de contingence: ", end="\n\n")    
    print(        
          pd.crosstab(            
          base[variable],            
          base["target"], #à transformer           
          normalize="index"        
          )    
          )    
    
    sns.countplot(
            x=variable,        
            data=base    
            )    
    plt.title(
            f"Répartition de {variable}\n",        
            fontsize=20    
            )    
    plt.show()
```

Utilisation :

```python
analyse_variable_binaire("Driving_License",train)
```

## Ce qu’elle retourne

Elle **n’utilise pas `return`** : elle affiche directement :

```
répartition
+
tableau de contingence avec la cible
+
countplot
```

### Vigilance

Le tableau de contingence est ici **descriptif**.

```python
pd.crosstab(...)
```

ne constitue pas à lui seul un test statistique de dépendance.

---

# 2. `analyse_variable_quantitative()`

## Quand l’utiliser ?

Pour obtenir rapidement sur une variable quantitative :

- effectif ;
- moyenne ;
- écart-type ;
- quartiles ;
- min/max ;
- boxplot ;
- premières indications sur les valeurs extrêmes.

## Pattern du cours

```python
def analyse_variable_quantitative(variable, base):    
    print()    
    print(
            "Statistiques de la variable: ",        
            end="\n\n"    
            )    
    print(
            base[variable].describe(),        
            end="\n\n"    
            )    
    sns.boxplot(
            x=variable,        
            data=base    
            )    
    plt.title(
            f"Distribution de {variable}\n",        
            fontsize=20    
            )    
    plt.show()
```

Utilisation :

```python
analyse_variable_quantitative("Annual_Premium",train)
```

## Ce qu’elle permet de repérer

```
describe()
→ position + dispersion + bornes

boxplot
→ dispersion + valeurs extrêmes potentielles
```

### Vigilance

Un point situé hors des moustaches du boxplot est une **valeur extrême statistique**, pas automatiquement une erreur.


---

# 3. `separation()`

## Quand l’utiliser ?

Une fois la préparation terminée, pour séparer :

```
DataFrame
→ X : variables explicatives
→ y : cible
```

La fonction originale conserve également une copie de la base complète. Texte collé (3)

## Version du cours

```python
def separation(df):
    base = df.copy()    
    y = df["Response"]    
    X = df.drop(
            ["target", "variable"],#a modifier selon le dataset        
            axis=1    
            )    
    return base, X, y
#ou 
def separation(df, target, drop_cols=None):
    base = df.copy()
    if drop_cols is None:
        drop_cols = []    
    y = df[target].copy()    
    X = df.drop(
        columns=[target] + drop_cols    
        ).copy()    
    return base, X, y
```

Utilisation :

```python
train_base, X_train, y_train = separation(train)

train_base, X_train, y_train = separation(df=train,target="Response", drop_cols=["Vintage"])
```

### Sorties

```
base
→ copie complète

X
→ variables explicatives

y
→ cible
```

---

# 4. `traitement()` — pattern de preprocessing complet

## Quand l’utiliser ?

Lorsque plusieurs transformations doivent être appliquées **de manière reproductible** à de nouvelles données.

Le cours regroupe dans une seule fonction :

- index ;
- suppression de variables ;
- imputation ;
- regroupement de modalités ;
- transformation des dates ;
- création de variable ;
- scaling ;
- suppression de variables devenues inutiles ;
- séparation `X / y`. Texte collé (3)

## Structure à retenir

Ne pas copier les règles assurance. Retenir **l’architecture** :

```python
def traitement(df):
    df = df.copy()    
    # 1. Structure    
    # index / colonnes inutiles    
    
    # 2. Données manquantes    
    # imputation    
    
    # 3. Variables catégorielles    
    # encodage / regroupement    
    
    # 4. Dates    
    # conversion / création de variables    
    
    # 5. Variables quantitatives    
    # transformations / scaling    
    
    # 6. Nettoyage final    
    # suppression des variables devenues inutiles    
    
    # 7. Séparation X / y    
    return base, X, y
```

## Pourquoi ce pattern est intéressant ?

Il transforme une succession de cellules :

```
traitement 1
traitement 2
traitement 3
traitement 4
...
```

en une opération reproductible :

```
base, X, y = traitement(df)
```

C’est particulièrement utile lorsqu’il faut appliquer **exactement les mêmes règles** à plusieurs jeux de données.

---

## ⚠️ Vigilance majeure : scaler et apprentissage

La fonction `traitement()` du cours contient volontairement une simplification incorrecte : elle recrée les scalers puis fait un `fit_transform()` sur le test. Le cours le signale lui-même comme une mauvaise pratique. 

La logique correcte est :

```python
# TRAIN
scaler.fit(X_train)
X_train_scaled = scaler.transform(X_train)

# TEST
X_test_scaled = scaler.transform(X_test)
```

Donc :

```
TRAIN
→ apprend les paramètres

TEST / nouvelles données
→ reçoit les paramètres appris
```

Ne jamais :

```
scaler.fit_transform(X_test)
```

dans un vrai pipeline d’évaluation.



