# Label, Ordinal et Thermometer Encoding

## Syntaxe

### Label Encoding : cible `y`
```python
from sklearn.preprocessing import LabelEncoder
enc_y = LabelEncoder()
y_train_enc = enc_y.fit_transform(y_train)
y_test_enc = enc_y.transform(y_test)
```

### Ordinal Encoding : variable explicative ordonnée
```python
from sklearn.preprocessing import OrdinalEncoder
enc = OrdinalEncoder(categories=[['Faible', 'Moyen', 'Élevé']],
                     handle_unknown='use_encoded_value', unknown_value=-1)
X_train_enc = enc.fit_transform(X_train[['Priorite']])
X_test_enc = enc.transform(X_test[['Priorite']])
```

### Thermometer Encoding : représentation cumulative (exemple maison)
```python
import numpy as np
import pandas as pd
ordre = ['Faible', 'Moyen', 'Élevé']
def thermometer(serie):
    codes = pd.Categorical(serie, categories=ordre, ordered=True).codes
    if (codes < 0).any():
        raise ValueError('Catégorie inconnue ou manquante')
    return (codes[:, None] >= np.arange(len(ordre))).astype(int)
X_train_enc = thermometer(X_train['Priorite'])
X_test_enc = thermometer(X_test['Priorite'])
```

## Comparaison
| Méthode | Entrée | Colonnes | Usage |
|---|---|---|---|
| Label | Cible catégorielle `y` | 1 | Classes de classification |
| Ordinal | `X` ordonnée | 1 | Ordre métier réel |
| Thermometer | `X` ordonnée | k (ou k−1 selon convention) | Seuils cumulatifs |

## Fonction
Label code les classes de `y` ; Ordinal représente l'ordre de `X` ; Thermometer encode cumulativement cet ordre (`Faible=100`, `Moyen=110`, `Élevé=111`).

## Vigilances & tips
- `LabelEncoder` est destiné à `y`, pas aux colonnes explicatives.
- Un code ordinal n'implique pas que les distances entre niveaux soient égales.
- Définir l'ordre explicitement. Les catégories inconnues demandent un traitement prévu.
- La convention Thermometer choisie ici crée k colonnes ; d'autres utilisent k−1.
