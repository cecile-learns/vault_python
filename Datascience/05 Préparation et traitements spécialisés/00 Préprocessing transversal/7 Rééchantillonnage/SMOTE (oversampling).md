
# SMOTE — Synthetic Minority Over-sampling Technique

## Syntaxe

```python
from imblearn.over_sampling import SMOTE

smote = SMOTE(
    sampling_strategy="auto",
    k_neighbors=5,
    random_state=42
)

X_train_res, y_train_res = smote.fit_resample(
    X_train, y_train
)
```

## Paramètres

- `sampling_strategy="auto"` : augmente les classes minoritaires jusqu'à l'effectif majoritaire.
    
- `sampling_strategy=0.5` : en classification binaire, vise un ratio minoritaire/majoritaire de 0,5.
    
- `sampling_strategy={1: 500}` : fixe l'effectif final de la classe `1`.
    
- `k_neighbors=5` : nombre de voisins minoritaires utilisés pour générer les observations synthétiques.
    
- `random_state=42` : rend la génération reproductible.
    

## Fonction

SMOTE équilibre les classes en **créant de nouvelles observations synthétiques par interpolation entre des observations minoritaires proches**.

Principe :

```
Observation A (minoritaire)
        ↓
Recherche de voisins minoritaires
        ↓
Choix d'un voisin B
        ↓
Création d'un point entre A et B
```

Formule :

`X_nouveau = A + λ × (B - A)`

avec `λ` tiré aléatoirement entre 0 et 1.

## Exemple

Deux observations minoritaires :

- A = (2, 4)
    
- B = (6, 8)
    

Avec `λ = 0.5` :

`X_nouveau = (4, 6)`

La nouvelle observation se situe entre A et B.

## Vigilances & tips

- **Variables numériques** : SMOTE classique utilise des distances et des interpolations. Pour des variables catégorielles, envisager `SMOTENC` (variables mixtes) ou `SMOTEN` (variables entièrement catégorielles).
    
- **Choix de** `**k_neighbors**` : il doit être inférieur au nombre d'observations disponibles dans chaque classe à suréchantillonner.
    
- **Valeurs aberrantes** : SMOTE peut créer des observations synthétiques autour de points atypiques.
    
- **Chevauchement des classes** : les nouveaux points peuvent accentuer le mélange entre classes.
    
- **Scaling** : les variables de grande amplitude peuvent dominer le calcul des distances.
    
- **Fuite de données** : appliquer SMOTE uniquement au train et à l'intérieur des folds de validation croisée.
    
- **Test final** : conserver la distribution réelle des classes.
    

**S'utilise souvent avec :** `StandardScaler`, `SMOTENC`, `Pipeline` (imblearn), `StratifiedKFold`.