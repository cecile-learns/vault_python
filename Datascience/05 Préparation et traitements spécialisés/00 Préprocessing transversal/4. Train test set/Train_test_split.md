

# `train_test_split`

## Quand l’utiliser ?

Séparer un jeu de données en plusieurs sous-ensembles, généralement :

```
train
→ apprendre le modèle

test
→ évaluer sa capacité à généraliser
```

Utilisable en classification comme en régression.

## Principe

`train_test_split()` applique **le même découpage** à `X` et `y`.

```
X, y
↓
même tirage aléatoire
↓
X_train   X_test
y_train   y_test
```

Les lignes restent alignées :

```
X_train[i] ↔ y_train[i]
X_test[i]  ↔ y_test[i]
```

![[Pasted image 20261006135047.png]]


## Pattern minimal

``` python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(    
                         X,    
                         y,    
                        test_size=0.2,    
                        random_state=42)
```

## Paramètres utiles

`test_size` : Proportion réservée au test.
`random_state` : Fixe le tirage aléatoire pour obtenir le même découpage à chaque exécution.
`stratify` : Préserve approximativement la distribution d’une variable.
En classification : `stratify=y`, particulièrement utile si les classes sont déséquilibrées.
`shuffle` : par défaut True , les lignes sont mélangées avant la séparation. avec shuffle=False 
ne mélange pas les lignes avant de couper par conséquent, `stratify` ne peut pas être utilisé.

## Vérification rapide

```
print(X_train.shape)print(X_test.shape)print(y_train.shape)print(y_test.shape)
```

En classification :

```
print(y.value_counts(normalize=True))print(y_train.value_counts(normalize=True))print(y_test.value_counts(normalize=True))
```

Permet notamment de vérifier l’effet de `stratify`.

## Points de vigilance

- **Ne pas entraîner sur le test.**
- Les transformations apprises sur les données (`scaler`, imputation, encodage appris, etc.) doivent être ajustées sur le train puis appliquées au test.
- Pour des données temporelles, un découpage aléatoire peut être inadapté.
- Si plusieurs lignes appartiennent au même individu ou au même groupe, elles peuvent se retrouver des deux côtés et créer une fuite.
- `stratify=y` peut échouer si certaines classes contiennent trop peu d’observations.
- Fixer `random_state` garantit la reproductibilité, pas la représentativité.

## À retenir

```
train_test_split
→ même séparation pour X et y

test_size
→ taille du test

random_state
→ reproductibilité

stratify=y
→ conserver les proportions de classes

shuffle
→ mélanger avant séparation
```

```python
# Séparation des jeux
X=df.drop('class', axis=1)
y=df['class']

#Séparation du test
X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.15,
    random_state=42,
    stratify=y
)

#Séparation de la validation
X_train, X_val, y_train, y_val = train_test_split(
    X_train, y_train,
    test_size=0.18,  
    random_state=42,
    stratify=y_train
)
```
attention au second stratify qui se fait sur y_train et non y