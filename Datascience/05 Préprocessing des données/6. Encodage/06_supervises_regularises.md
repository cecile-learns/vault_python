# Leave One Out, James-Stein et M-estimator Encoding

## Syntaxe

### Leave One Out
```python
from category_encoders import LeaveOneOutEncoder
enc = LeaveOneOutEncoder(cols=['Secteur'], sigma=0.05, random_state=42)
X_train_enc = enc.fit_transform(X_train, y_train)
X_test_enc = enc.transform(X_test)
```

### James-Stein
```python
from category_encoders import JamesSteinEncoder
enc = JamesSteinEncoder(cols=['Secteur'], model='independent')
X_train_enc = enc.fit_transform(X_train, y_train)
X_test_enc = enc.transform(X_test)
```

### M-estimator
```python
from category_encoders import MEstimateEncoder
enc = MEstimateEncoder(cols=['Secteur'], m=5.0)
X_train_enc = enc.fit_transform(X_train, y_train)
X_test_enc = enc.transform(X_test)
```

## Comparaison
| Méthode | Principe | Colonnes |
|---|---|---|
| Leave One Out | Moyenne par catégorie sans la cible de la ligne encodée sur le train | 1 par variable |
| James-Stein | Rétrécissement des estimations des catégories vers une estimation globale | 1 par variable |
| M-estimator | Moyenne de cible lissée vers la moyenne globale | 1 par variable |

## Paramètres
- `cols` : colonnes catégorielles.
- `sigma` : bruit d'entraînement optionnel pour Leave One Out.
- `model` : variante du rétrécissement James-Stein.
- `m` : force du lissage pour M-estimator.

## Fonction
Encodages **supervisés** destinés à exploiter `y` en limitant les estimations trop instables, notamment sur des catégories rares.

## Vigilances & tips
- Le Leave One Out ne supprime pas tout risque de surapprentissage.
- Les mécanismes diffèrent : exclusion de la ligne, rétrécissement statistique, lissage.
- Le `fit_transform()` de ces encodeurs ne correspond pas nécessairement au cross-fitting interne de `sklearn.TargetEncoder`.
- Choisir selon la cible et valider les performances par CV ; `fit` train, `transform` test.
