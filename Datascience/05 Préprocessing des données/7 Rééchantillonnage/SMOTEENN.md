
# SMOTEENN — Suréchantillonnage et nettoyage par voisinage

## Syntaxe

```python
from imblearn.combine import SMOTEENN

sampler = SMOTEENN(
    sampling_strategy="auto",
    random_state=42
)

X_train_res, y_train_res = sampler.fit_resample(
    X_train, y_train
)
```

## Paramètres

- `sampling_strategy="auto"` : définit les classes à suréchantillonner et leurs effectifs cibles.
    
- `sampling_strategy=0.5` : en classification binaire, demande à SMOTE un ratio minoritaire/majoritaire de 0,5 avant nettoyage.
    
- `random_state=42` : rend la génération reproductible.
    
- `smote` : permet de personnaliser SMOTE.
    
- `enn` : permet de personnaliser Edited Nearest Neighbours.
    

### Personnalisation

```python
from imblearn.over_sampling import SMOTE
from imblearn.under_sampling import EditedNearestNeighbours
from imblearn.combine import SMOTEENN

sampler = SMOTEENN(
    smote=SMOTE(k_neighbors=3, random_state=42),
    enn=EditedNearestNeighbours(
        n_neighbors=3,
        kind_sel="all"
    ),
    random_state=42
)
```

## Fonction

`SMOTEENN` combine deux opérations successives :

1. **SMOTE (oversampling)** : crée des observations synthétiques minoritaires.
    
2. **Edited Nearest Neighbours (undersampling)** : examine le voisinage des observations et supprime celles qui ne respectent pas la règle de cohérence choisie.
    

Principe :

```
Dataset déséquilibré
        ↓
SMOTE
→ Création d'observations minoritaires
        ↓
Edited Nearest Neighbours
→ Suppression d'observations
  incohérentes avec leur voisinage
        ↓
Dataset rééchantillonné et nettoyé
```

### Exemple

|Classe|Avant|Après SMOTE|
|---|---|---|
|Majoritaire|900|900|
|Minoritaire|100|900|

ENN examine ensuite les voisinages.

Selon la répartition des données, il peut supprimer des observations majoritaires et minoritaires, y compris certaines observations synthétiques.

**Les effectifs finaux sont déterminés par le nettoyage**, et non uniquement par `sampling_strategy`.

## Vigilances & tips

- **Nettoyage plus étendu** : ENN peut supprimer davantage d'observations que Tomek Links, notamment dans les zones où les classes se chevauchent.
    
- **Règle de voisinage** : avec `kind_sel="all"`, une observation ciblée doit être cohérente avec tous ses voisins pour être conservée.
    
- **Classes concernées** : par défaut, le nettoyage ENN de `SMOTEENN` peut concerner toutes les classes.
    
- **Perte d'information** : des observations difficiles mais pertinentes peuvent être supprimées.
    
- **Observations synthétiques** : certaines peuvent être supprimées immédiatement après leur création par SMOTE.
    
- **Équilibrage final non garanti** : ENN peut modifier fortement les effectifs.
    
- **Distances et scaling** : les deux étapes reposent sur des distances ; adapter l'échelle des variables.
    
- **Variables catégorielles** : la version classique convient principalement aux variables numériques.
    
- **Fuite de données** : appliquer uniquement dans les folds d'entraînement.
    
- **Test final** : conserver les observations et proportions réelles.
    

**S'utilise souvent avec :** `SMOTE`, `EditedNearestNeighbours`, `StandardScaler`, `Pipeline` (imblearn), `StratifiedKFold`.