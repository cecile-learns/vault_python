# RandomUnderSampler — Sous-échantillonnage aléatoire

## Syntaxe

```python
from imblearn.under_sampling import RandomUnderSampler

rus = RandomUnderSampler(
    sampling_strategy="auto",
    random_state=42,
    replacement=False
)

X_train_res, y_train_res = rus.fit_resample(
    X_train, y_train
)
```

## Paramètres

- `sampling_strategy="auto"` : réduit les classes majoritaires jusqu'à l'effectif de la classe minoritaire.
    
- `sampling_strategy=0.5` : en classification binaire, vise un ratio minoritaire/majoritaire de 0,5 après rééchantillonnage.
    
- `sampling_strategy={0: 300}` : fixe à 300 observations l'effectif final de la classe `0`.
    
- `random_state=42` : rend le tirage reproductible.
    
- `replacement=False` : sélectionne les observations sans remise (une observation ne peut pas être tirée plusieurs fois).
    

## Fonction

`RandomUnderSampler` est une méthode de **sous-échantillonnage (**_**undersampling**_**)** qui réduit les classes majoritaires en supprimant aléatoirement certaines de leurs observations.

Contrairement à `ClusterCentroids`, il ne cherche pas à représenter la structure des données : il conserve simplement un échantillon aléatoire des observations existantes.

### Exemple

Dataset initial :

|Classe|Avant|Après|
|---|---|---|
|Majoritaire|900|100|
|Minoritaire|100|100|

Avec `sampling_strategy="auto"`, 800 observations majoritaires sont supprimées aléatoirement.

Principe :

```
900 observations majoritaires
           ↓
   Tirage aléatoire
           ↓
100 observations conservées
           +
100 observations minoritaires
           ↓
200 observations au total
```

## Comparaison avec les autres méthodes

|Méthode|Famille|Principe|
|---|---|---|
|`RandomOverSampler`|Oversampling|Duplique les observations minoritaires|
|`SMOTE`|Oversampling|Crée des observations minoritaires synthétiques|
|`ADASYN`|Oversampling|Génère davantage de points dans les zones difficiles|
|`ClusterCentroids`|Undersampling|Représente les classes majoritaires par des centroïdes ou des observations proches|
|`RandomUnderSampler`|Undersampling|Supprime aléatoirement des observations majoritaires|

## Vigilances & tips

- **Perte d'information** : les observations supprimées peuvent contenir des informations utiles à la classification.
    
- **Tirage aléatoire** : ne tient pas compte de la structure des données ni de la proximité entre classes.
    
- **Rapidité** : méthode simple et peu coûteuse en calcul, adaptée notamment aux grands datasets.
    
- **Variables catégorielles** : fonctionne avec des variables numériques et catégorielles, car aucun calcul de distance n'est nécessaire.
    
- **Déséquilibre important** : un rééquilibrage complet peut supprimer une très grande partie des données ; envisager un ratio intermédiaire.
    
- **Reproductibilité** : fixer `random_state` pour retrouver le même échantillon.
    
- **Fuite de données** : appliquer le sous-échantillonnage uniquement sur le train et à l'intérieur des folds d'entraînement lors d'une validation croisée.
    
- **Test final** : conserver la distribution réelle des classes pour évaluer la généralisation.
    

**S'utilise souvent avec :** `Pipeline` (imblearn), `StratifiedKFold`, `classification_report`, `balanced_accuracy_score`.