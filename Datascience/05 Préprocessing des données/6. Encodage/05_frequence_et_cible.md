# Frequency, Target, Mean, WOE et Probability Ratio Encoding

## Syntaxe

### Frequency Encoding : fréquence, sans cible
```python
frequences = X_train['Secteur'].value_counts(normalize=True)
X_train_enc = X_train.copy()
X_test_enc = X_test.copy()
for X_enc in [X_train_enc, X_test_enc]:
    X_enc['Secteur_freq'] = X_enc['Secteur'].map(frequences).fillna(0)
    X_enc.drop(columns='Secteur', inplace=True)
```

### Target Encoding (scikit-learn) : statistiques de la cible
```python
from sklearn.preprocessing import TargetEncoder
enc = TargetEncoder(target_type='binary', smooth='auto', cv=5, random_state=42)
X_train_enc = enc.fit_transform(X_train[['Secteur']], y_train)
X_test_enc = enc.transform(X_test[['Secteur']])
```

### Mean Encoding : cas particulier du Target Encoding
```python
# Pour une cible quantitative : moyenne de y par catégorie, avec lissage/cross-fitting
enc = TargetEncoder(target_type='continuous', cv=5, random_state=42)
X_train_enc = enc.fit_transform(X_train[['Secteur']], y_train)
X_test_enc = enc.transform(X_test[['Secteur']])
```

### Weight of Evidence (WOE) : cible binaire
```python
from category_encoders import WOEEncoder
enc = WOEEncoder(cols=['Secteur'], regularization=1.0)
X_train_enc = enc.fit_transform(X_train, y_train)
X_test_enc = enc.transform(X_test)
```

### Probability Ratio : principe, sans encodeur sklearn standard
```python
import pandas as pd
# Calcul illustratif sur train, PAS une procédure de cross-fitting.
table = pd.DataFrame({'categorie': X_train['Secteur'].to_numpy(),
                      'cible': y_train.to_numpy()})
p = table.groupby('categorie')['cible'].mean()  # y binaire 0/1
ratio = (p + 1e-6) / (1 - p + 1e-6)
```

## Comparaison
| Méthode | Utilise `y` ? | Cible | Colonnes |
|---|---|---|---|
| Frequency | Non | Indifférente | 1 |
| Target / Mean | Oui | Continue, binaire, multiclasse selon encodeur | 1 ou plusieurs (multiclasse) |
| WOE | Oui | Binaire | 1 |
| Probability Ratio | Oui | Binaire | 1 |

## Fonction
Remplacer une modalité par sa **fréquence** ou par une **statistique de la cible**. Mean Encoding et Target Encoding se recouvrent largement.

## Vigilances & tips
- **Frequency n'est pas supervisé** : il n'utilise pas `y`.
- Le WOE prend le logarithme d'un rapport de probabilités/odds ; le Probability Ratio non. Le signe du WOE dépend de la convention.
- Catégories rares : estimations instables, besoin de lissage.
- **Fuite de cible** : calculer la moyenne de `y_train` par catégorie et la réinjecter sur ces mêmes lignes favorise le surapprentissage.
- `sklearn.TargetEncoder.fit_transform()` réalise du **cross-fitting** interne, contrairement à `fit().transform()` sur le train.
- `category_encoders.WOEEncoder` ne garantit pas ce même cross-fitting interne : organiser la validation en conséquence.
- Ne jamais apprendre les statistiques sur le test.
