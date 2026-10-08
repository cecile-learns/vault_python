
# Séparer une base en `train` / `test` avec `pandas.sample`

## Quand l’utiliser ?

Isoler très tôt une **base d’entraînement** et une **base de test brute**, avant l’exploration approfondie, le nettoyage et le feature engineering.

C’est utile quand on veut travailler ainsi :

```
base complète
↓
train
test
↓
travail uniquement sur train
↓
évaluation finale sur test
```

## Principe

Avec `pandas.sample()`, on tire aléatoirement une partie des lignes pour constituer le test.

Puis on récupère les lignes restantes pour le train.

```
df complet
↓
sample()
↓
test

df - lignes du test
↓
train
```

Ici, on sépare des **observations complètes**.

À ce stade, on ne sépare pas encore forcément : X et y
La cible peut donc rester présente dans `train` et `test`.

## Pattern minimal

```Python
# 10 % des lignes pour le test
test = df.sample(    frac=0.10,    random_state=42)
test.to_csv('test_copy.csv', index=False)
# Toutes les autres lignes pour le train
train = df.drop(test.index)
train.to_csv('train_copy.csv', index=False)
```


## Paramètres utiles

`frac` : Proportion de lignes à tirer.
 `n` : Nombre exact de lignes à tirer.
`random_state` : Permet de reproduire le même tirage
`replace` : Par défaut, replace=False une ligne ne peut être tirée qu'une seule fois

## Vérifier la séparation

```
print(train.shape)print(test.shape)
```

Vérifier aussi qu’aucune ligne n’est présente dans les deux :

```
assert train.index.intersection(test.index).empty
```

Et que tout le dataset est bien réparti :

```
assert len(train) + len(test) == len(df)
```

## Après la séparation

Travailler uniquement sur `train` pour :

```
EDA
→ traitement des NaN
→ outliers
→ encodage
→ feature engineering
→ scaling
→ choix des variables
→ modélisation / validation
```

Le `test` reste à l’écart jusqu’à l’évaluation finale.

## Différence avec `train_test_split`

```
pd.sample()
→ on crée directement deux DataFrames complets
→ pratique pour un workflow EDA / preprocessing

train_test_split()
→ souvent utilisé pour produire directement
  X_train, X_test, y_train, y_test
```

Les deux peuvent servir à faire une séparation train/test.

Ici, `sample()` est surtout utilisé parce que le cours veut conserver deux **bases complètes**, puis retarder la séparation `X / y`.

---

## Points de vigilance

- `sample()` ne fait pas de stratification automatique.
- Si les classes sont très déséquilibrées, vérifier leur proportion dans `train` et `test`.
- Ne pas faire d’EDA approfondie sur le test.
- Ne pas apprendre un scaler, un encodeur ou une imputation sur le test.
- Pour des données temporelles, un tirage aléatoire peut être inadapté.
- Si plusieurs lignes appartiennent au même individu, elles peuvent être séparées entre train et test.

---

## À retenir

```
test = df.sample(frac=...)
train = df.drop(test.index)
```

Puis :

```
train
→ apprendre / explorer / transformer

test
→ garder à l’écart pour l’évaluation finale
```

C’est une séparation des **lignes du dataset**, pas encore une séparation entre **features `X` et cible `y`**.