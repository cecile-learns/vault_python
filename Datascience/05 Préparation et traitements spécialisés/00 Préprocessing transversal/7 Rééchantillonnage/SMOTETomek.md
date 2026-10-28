# SMOTETomek — Suréchantillonnage et nettoyage des frontières

## Syntaxe

```python
from imblearn.combine import SMOTETomek

sampler = SMOTETomek(
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
    
- `smote` : permet de personnaliser l'algorithme SMOTE utilisé.
    
- `tomek` : permet de personnaliser le nettoyage par Tomek Links.
    

### Personnalisation

```python
from imblearn.over_sampling import SMOTE
from imblearn.under_sampling import TomekLinks
from imblearn.combine import SMOTETomek

sampler = SMOTETomek(
    smote=SMOTE(k_neighbors=3, random_state=42),
    tomek=TomekLinks(sampling_strategy="all"),
    random_state=42
)
```

## Fonction

`SMOTETomek` combine deux opérations successives :

1. **SMOTE (oversampling)** : crée des observations synthétiques de la classe minoritaire par interpolation entre voisins.
    
2. **Tomek Links (undersampling)** : identifie des paires d'observations de classes différentes, mutuellement plus proches voisines, puis supprime les observations ciblées.
    

Principe :

```
Dataset déséquilibré
        ↓
SMOTE
→ Création d'observations minoritaires
        ↓
Tomek Links
→ Nettoyage de certaines frontières
        ↓
Dataset rééchantillonné
```

### Exemple

|Classe|Avant|Après SMOTE|
|---|---|---|
|Majoritaire|900|900|
|Minoritaire|100|900|

Tomek Links intervient ensuite pour supprimer certaines observations.

**Les effectifs finaux dépendent des liens détectés** et peuvent donc différer de 900/900.

## Vigilances & tips

- **Nettoyage ciblé** : Tomek Links retire des observations situées à certaines frontières entre classes, pas nécessairement toutes les observations ambiguës.
    
- **Suppression des classes** : par défaut, le nettoyage vise les classes majoritaires ; `sampling_strategy="all"` permet de supprimer les deux membres des liens détectés.
    
- **Équilibrage final non garanti** : le nettoyage intervient après SMOTE et peut modifier les effectifs.
    
- **Distances** : SMOTE et Tomek Links reposent sur la proximité entre observations ; un scaling adapté est souvent nécessaire.
    
- **Variables catégorielles** : la version classique convient principalement aux variables numériques.
    
- **Fuite de données** : appliquer uniquement sur le train, à l'intérieur des folds de validation croisée.
    
- **Test final** : ne jamais rééchantillonner le test.
    

**S'utilise souvent avec :** `SMOTE`, `TomekLinks`, `StandardScaler`, `Pipeline` (imblearn), `StratifiedKFold`.