# ADASYN — Adaptive Synthetic Sampling

## Syntaxe

```python
from imblearn.over_sampling import ADASYN

adasyn = ADASYN(
    sampling_strategy="auto",
    n_neighbors=5,
    random_state=42
)

X_train_res, y_train_res = adasyn.fit_resample(
    X_train, y_train
)
```

## Paramètres

- `sampling_strategy="auto"` : vise le rééquilibrage des classes minoritaires.
    
- `sampling_strategy=0.5` : en classification binaire, vise un ratio minoritaire/majoritaire de 0,5.
    
- `sampling_strategy={1: 500}` : définit l'effectif final souhaité pour la classe `1`.
    
- `n_neighbors=5` : nombre de voisins utilisés pour évaluer la difficulté locale et générer les observations.
    
- `random_state=42` : rend la génération reproductible.
    

## Fonction

ADASYN crée des observations synthétiques comme SMOTE, mais **adapte leur quantité à la difficulté locale de classification**.

Les observations minoritaires entourées de nombreuses observations majoritaires reçoivent davantage de nouveaux exemples synthétiques.

Principe :

```
Observation minoritaire
        ↓
Analyse des voisins
        ↓
Beaucoup de voisins majoritaires ?
        ↓
Oui → davantage de points synthétiques
Non → moins de points synthétiques
```

L'objectif est de renforcer l'apprentissage dans les zones où la classe minoritaire est difficile à distinguer.

## Exemple

|Zone|Voisins majoritaires|Nouveaux points|
|---|---|---|
|A : minoritaires bien regroupés|Peu|Peu|
|B : classes mélangées|Nombreux|Davantage|

La quantité exacte dépend de la distribution des données et du paramétrage.

## Vigilances & tips

- **Différence avec SMOTE** : SMOTE répartit plus uniformément la génération entre les observations minoritaires ; ADASYN privilégie les zones localement difficiles.
    
- **Bruit et outliers** : ADASYN peut amplifier les observations aberrantes ou les zones très ambiguës.
    
- **Frontière de décision** : peut aider à mieux représenter certaines zones difficiles, sans garantie d'amélioration.
    
- **Variables numériques** : les distances doivent être pertinentes ; les variables catégorielles brutes ne conviennent pas.
    
- **Scaling** : généralement important avant le calcul des voisins.
    
- **Nombre d'observations** : l'effectif final peut différer légèrement de la quantité demandée.
    
- **Fuite de données** : appliquer ADASYN uniquement dans les folds d'entraînement.
    
- **Test final** : ne jamais rééchantillonner le jeu de test.
    

**S'utilise souvent avec :** `StandardScaler`, `Pipeline` (imblearn), `StratifiedKFold`, `classification_report`.