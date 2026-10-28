# Encodages cycliques : trigonométrique et bases radiales

## Syntaxe

### Trigonométrique : sinus/cosinus
```python
import numpy as np

def encoder_mois(df):
    resultat = df.copy()
    angle = 2 * np.pi * (resultat['Mois'] - 1) / 12
    resultat['Mois_sin'] = np.sin(angle)
    resultat['Mois_cos'] = np.cos(angle)
    return resultat.drop(columns='Mois')

X_train_enc = encoder_mois(X_train)
X_test_enc = encoder_mois(X_test)
```

### Bases radiales périodiques (RBF)
```python
import numpy as np

def encoder_mois_rbf(df, centres=range(12), gamma=0.5):
    resultat = df.drop(columns='Mois').copy()
    mois = df['Mois'].to_numpy() - 1
    for centre in centres:
        distance = np.abs(mois - centre)
        distance = np.minimum(distance, 12 - distance)
        resultat[f'Mois_rbf_{centre}'] = np.exp(-gamma * distance**2)
    return resultat

X_train_enc = encoder_mois_rbf(X_train)
X_test_enc = encoder_mois_rbf(X_test)
```

## Comparaison
| Méthode | Colonnes | Quand choisir ? |
|---|---|---|
| Sinus/cosinus | 2 | Cycle représenté de façon compacte |
| RBF périodiques | Nombre de centres choisi | Effets locaux souples selon le moment du cycle |

## Paramètres
- `12` : période en mois (adapter à 24 h, 7 jours, etc.).
- `centres` : positions des bases radiales ; leur nombre fixe la dimension.
- `gamma` : largeur des bases ; plus élevé = effets plus localisés.

## Fonction
Représenter le **retour du cycle sur lui-même** : décembre est proche de janvier, 23 h de 0 h.

## Vigilances & tips
- Le One-Hot ne représente pas explicitement cette proximité.
- Vérifier la période et l'origine du codage (`Mois - 1`).
- Les RBF ici utilisent une **distance circulaire**, pas linéaire.
- Transformations indépendantes de `y`, identiques sur train et test.
- **S'utilise souvent avec :** `FunctionTransformer`, `ColumnTransformer`, `Pipeline`.
