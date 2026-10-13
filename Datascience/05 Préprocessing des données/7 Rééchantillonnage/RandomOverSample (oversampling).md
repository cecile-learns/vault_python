
# RandomOverSampler — Suréchantillonnage aléatoire

## Syntaxe

```python
from imblearn.over_sampling import RandomOverSampler

ros = RandomOverSampler(
    sampling_strategy="auto",
    random_state=42
)

X_train_res, y_train_res = ros.fit_resample(
    X_train, y_train
)
```

## Paramètres

- `sampling_strategy="auto"` : augmente les classes minoritaires jusqu'à l'effectif de la classe majoritaire.
    
- `sampling_strategy=0.5` : en classification binaire, fixe l'effectif minoritaire à 50 % de l'effectif majoritaire après rééchantillonnage.
    
- `sampling_strategy={1: 500}` : fixe explicitement à 500 observations l'effectif final de la classe `1`.
    
- `random_state=42` : rend le tirage reproductible.
    
- `shrinkage` : permet éventuellement de générer des copies légèrement perturbées pour les données numériques.
    

## Fonction

`RandomOverSampler` équilibre les classes en **dupliquant aléatoirement des observations de la classe minoritaire**.

Exemple :

|Classe|Avant|Après|
|---|---|---|
|Majoritaire|900|900|
|Minoritaire|100|900|

Avec `sampling_strategy="auto"`, les 800 observations supplémentaires sont obtenues par tirage avec remise parmi les 100 observations minoritaires.

## Vigilances & tips

- Ne crée pas de nouvelles informations : les observations sont dupliquées.
    
- Peut favoriser le surapprentissage si le modèle mémorise les observations répétées.
    
- Fonctionne avec des variables numériques et catégorielles.
    
- Ne pas appliquer avant la séparation train/test.
    
- En validation croisée, réaliser le rééchantillonnage **uniquement dans les folds d'entraînement**, idéalement avec `imblearn.pipeline.Pipeline`.
    
- Ne pas rééquilibrer artificiellement le jeu de test.
    

**S'utilise souvent avec :** `Pipeline` (imblearn), `StratifiedKFold`, `classification_report`.