
# Undersampling — Méthodes de sélection et de nettoyage

## Syntaxe générale

```python
from imblearn.under_sampling import (
    CondensedNearestNeighbour,
    EditedNearestNeighbours,
    RepeatedEditedNearestNeighbours,
    AllKNN,
    InstanceHardnessThreshold,
    NearMiss,
    NeighbourhoodCleaningRule,
    OneSidedSelection,
    TomekLinks
)

# Exemple : nettoyage avec EditedNearestNeighbours
sampler = EditedNearestNeighbours(n_neighbors=3)

X_train_res, y_train_res = sampler.fit_resample(
    X_train, y_train
)
```

Toutes ces méthodes utilisent `fit_resample(X, y)`.

## Fonction

Ces méthodes appartiennent à la famille du **sous-échantillonnage (undersampling)**.

Contrairement à `RandomUnderSampler`, elles ne suppriment pas les observations de manière purement aléatoire. Elles utilisent des critères de voisinage, de classification ou de difficulté des observations pour sélectionner celles à conserver ou à supprimer.

Elles poursuivent deux objectifs différents :

- **Réduction** : conserver un sous-ensemble représentatif des données.
    
- **Nettoyage** : supprimer des observations ambiguës, bruitées ou situées aux frontières des classes.
    

**Attention :** les méthodes de nettoyage ne garantissent pas un équilibrage des effectifs.

## Comparaison des méthodes

|Méthode|Principe|Objectif principal|
|---|---|---|
|`CondensedNearestNeighbour` (CNN)|Construit progressivement un sous-ensemble permettant de conserver les décisions d'un classifieur 1-NN|Réduction|
|`EditedNearestNeighbours` (ENN)|Supprime des observations dont le voisinage ne confirme pas suffisamment la classe|Nettoyage|
|`RepeatedEditedNearestNeighbours` (RENN)|Répète ENN jusqu'à un critère d'arrêt|Nettoyage itératif|
|`AllKNN`|Applique successivement ENN avec un nombre croissant de voisins|Nettoyage progressif|
|`InstanceHardnessThreshold` (IHT)|Utilise des probabilités prédites pour estimer la difficulté des observations|Sélection selon la difficulté|
|`NearMiss`|Sélectionne des observations majoritaires selon leurs distances aux minoritaires|Réduction orientée frontière|
|`NeighbourhoodCleaningRule` (NCR)|Combine ENN et une règle de nettoyage fondée sur les voisins|Nettoyage|
|`OneSidedSelection` (OSS)|Combine Condensed Nearest Neighbour et Tomek Links|Réduction et nettoyage|
|`TomekLinks`|Détecte des paires de classes différentes qui sont mutuellement plus proches voisines|Nettoyage des frontières|

## Paramètres et syntaxes spécifiques

### 1. CondensedNearestNeighbour

```
sampler = CondensedNearestNeighbour(
    n_neighbors=1,
    random_state=42
)
```

- `n_neighbors` : nombre de voisins utilisé par le classifieur interne.
    
- `random_state` : reproductibilité de la sélection.
    

Construit un sous-ensemble en ajoutant notamment les observations mal classées par le modèle de voisinage courant.

### 2. EditedNearestNeighbours

```
sampler = EditedNearestNeighbours(
    n_neighbors=3,
    kind_sel="all"
)
```

- `n_neighbors=3` : nombre de voisins considérés.
    
- `kind_sel="all"` : exige que tous les voisins confirment la classe pour conserver l'observation.
    
- `kind_sel="mode"` : utilise la classe majoritaire parmi les voisins.
    

### 3. RepeatedEditedNearestNeighbours

```
sampler = RepeatedEditedNearestNeighbours(
    n_neighbors=3,
    max_iter=100
)
```

- `n_neighbors` : nombre de voisins utilisés.
    
- `max_iter` : nombre maximal de répétitions.
    

Répète le nettoyage jusqu'à stabilisation ou jusqu'à une condition d'arrêt.

### 4. AllKNN

```
sampler = AllKNN(
    n_neighbors=3,
    kind_sel="all"
)
```

- `n_neighbors=3` : nombre maximal de voisins considéré.
    
- `kind_sel` : règle de conservation des observations.
    

Effectue plusieurs nettoyages en augmentant progressivement le nombre de voisins.

### 5. InstanceHardnessThreshold

```
sampler = InstanceHardnessThreshold(
    random_state=42,
    cv=5
)
```

- `estimator` : classifieur utilisé pour estimer les probabilités des classes.
    
- `cv=5` : validation croisée interne pour estimer ces probabilités.
    
- `random_state` : reproductibilité.
    

Conserve prioritairement les observations pour lesquelles le modèle attribue une probabilité élevée à la classe réelle.

### 6. NearMiss

```
sampler = NearMiss(
    version=1,
    n_neighbors=3
)
```

Trois variantes :

- `version=1` : sélectionne les observations majoritaires ayant la plus faible distance moyenne aux minoritaires les plus proches.
    
- `version=2` : sélectionne selon la distance moyenne aux minoritaires les plus éloignés.
    
- `version=3` : présélectionne des observations majoritaires proches des minoritaires, puis applique un critère de distance pour retenir les observations.
    

`n_neighbors` contrôle le voisinage utilisé dans les calculs de distance.

### 7. NeighbourhoodCleaningRule

```
sampler = NeighbourhoodCleaningRule(
    n_neighbors=3,
    threshold_cleaning=0.5
)
```

- `n_neighbors` : nombre de voisins utilisés.
    
- `threshold_cleaning` : seuil de taille relative des classes concernées par certaines suppressions.
    

Combine plusieurs règles pour nettoyer les observations difficiles et leurs voisinages.

### 8. OneSidedSelection

```
sampler = OneSidedSelection(
    n_neighbors=1,
    random_state=42
)
```

- `n_neighbors` : paramètre du classifieur de voisinage.
    
- `random_state` : reproductibilité.
    

Combine une sélection par Condensed Nearest Neighbour avec la suppression de liens de Tomek.

### 9. TomekLinks

```
sampler = TomekLinks(
    sampling_strategy="auto"
)
```

- `sampling_strategy="auto"` : supprime les observations appartenant aux classes ciblées, généralement majoritaires, impliquées dans des liens de Tomek.
    
- `sampling_strategy="all"` : autorise la suppression des deux observations d'un lien.
    

Un lien de Tomek correspond à deux observations de classes différentes qui sont mutuellement les plus proches voisines.

## Repère de choix

|Besoin|Méthodes à envisager|
|---|---|
|Nettoyer les observations ambiguës|ENN, RENN, AllKNN|
|Nettoyer les frontières des classes|TomekLinks, NCR|
|Réduire le nombre d'observations en conservant une structure de classification|CNN, OSS|
|Sélectionner selon la difficulté estimée par un modèle|IHT|
|Conserver des observations majoritaires proches des minoritaires|NearMiss|

Ces méthodes ne sont pas interchangeables : certaines cherchent à réduire fortement les effectifs, d'autres seulement à nettoyer les données.

## Vigilances & tips

- **Pas d'équilibrage garanti** : ENN, RENN, AllKNN, NCR et TomekLinks peuvent supprimer peu d'observations.
    
- **Perte d'information** : supprimer des observations difficiles peut également retirer des cas métier importants.
    
- **Distances** : la plupart des méthodes utilisent les plus proches voisins ; un scaling adapté est souvent nécessaire.
    
- **Variables catégorielles** : les distances euclidiennes ne sont généralement pas pertinentes sur des catégories brutes.
    
- **Bruit et frontières** : une observation difficile à classer n'est pas nécessairement erronée.
    
- **Coût de calcul** : les méthodes itératives ou fondées sur les voisins peuvent être coûteuses sur de grands datasets.
    
- `**sampling_strategy**` : les modalités acceptées et les classes ciblées dépendent de l'algorithme ; ne pas supposer qu'un ratio numérique est disponible partout.
    
- **Fuite de données** : appliquer ces méthodes uniquement au train et dans les folds d'entraînement.
    
- **Test final** : conserver la distribution réelle des classes.
    

**S'utilise souvent avec :** `StandardScaler`, `NearestNeighbors`, `Pipeline` (imblearn), `StratifiedKFold`, `classification_report`.