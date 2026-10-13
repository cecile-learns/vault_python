
# ClusterCentroids — Sous-échantillonnage par clustering

## Syntaxe

```python
from imblearn.under_sampling import ClusterCentroids

cc = ClusterCentroids(
    sampling_strategy="auto",
    random_state=42,
    voting="auto"
)

X_train_res, y_train_res = cc.fit_resample(
    X_train, y_train
)
```

## Paramètres

- `sampling_strategy="auto"` : réduit les classes majoritaires jusqu'à l'effectif de la classe minoritaire.
    
- `sampling_strategy=0.5` : en classification binaire, vise un ratio minoritaire/majoritaire de 0,5 après rééchantillonnage.
    
- `sampling_strategy={0: 300}` : fixe à 300 observations l'effectif final de la classe `0`.
    
- `random_state=42` : rend le clustering reproductible.
    
- `voting="auto"` : choisit automatiquement entre les deux stratégies de remplacement :
    
    - `"soft"` : utilise les centroïdes calculés par K-Means (observations synthétiques).
        
    - `"hard"` : sélectionne les observations réelles les plus proches des centroïdes.
        
- `estimator` : permet de fournir un algorithme de clustering compatible, généralement `KMeans`.
    

## Fonction

`ClusterCentroids` est une méthode de **sous-échantillonnage (**_**undersampling**_**)** qui réduit les classes majoritaires en utilisant un algorithme de clustering, généralement K-Means.

Au lieu de supprimer aléatoirement des observations, elle regroupe les observations majoritaires en clusters et conserve une représentation de chaque groupe.

### Exemple

Dataset initial :

|Classe|Avant|Après|
|---|---|---|
|Majoritaire|900|100|
|Minoritaire|100|100|

Avec `sampling_strategy="auto"`, les 900 observations majoritaires sont représentées par 100 centroïdes ou observations représentatives.

Principe :

```
900 observations majoritaires
           ↓
     K-Means (100 clusters)
           ↓
100 centroïdes représentatifs
           ↓
100 majoritaires + 100 minoritaires
```

## Comparaison avec les autres méthodes

|Méthode|Famille|Principe|
|---|---|---|
|`RandomOverSampler`|Oversampling|Duplique les observations minoritaires|
|`SMOTE`|Oversampling|Crée des observations minoritaires par interpolation|
|`ADASYN`|Oversampling|Crée davantage d'observations dans les zones difficiles|
|`ClusterCentroids`|Undersampling|Remplace ou sélectionne des représentants des classes majoritaires|

## Vigilances & tips

- **Perte d'information** : le sous-échantillonnage réduit le nombre d'observations disponibles pour l'entraînement.
    
- **Centroïdes synthétiques** : avec `voting="soft"`, les centroïdes ne correspondent pas nécessairement à des observations réelles.
    
- **Variables numériques** : la méthode repose sur des distances ; les variables catégorielles brutes ne conviennent généralement pas.
    
- **Scaling** : recommandé lorsque les variables présentent des échelles différentes, car K-Means utilise les distances.
    
- **Forme des clusters** : K-Means représente surtout des groupes compacts ; il peut mal résumer des distributions complexes.
    
- **Valeurs aberrantes** : les outliers peuvent influencer les positions des centroïdes.
    
- **Temps de calcul** : le clustering peut être coûteux sur de grands datasets.
    
- **Fuite de données** : appliquer le sous-échantillonnage uniquement sur le train et dans les folds d'entraînement lors d'une validation croisée.
    
- **Test final** : conserver la distribution réelle des classes pour mesurer la généralisation.
    

**S'utilise souvent avec :** `StandardScaler`, `KMeans`, `Pipeline` (imblearn), `StratifiedKFold`, `classification_report`.