
# `TimeSeriesSplit()` — Validation croisée temporelle

## Fonction

Découpe des données chronologiques en plusieurs ensembles d'entraînement et de test en **respectant l'ordre temporel**.

À chaque itération, l'entraînement utilise les observations passées et le test porte sur des observations ultérieures.

## Syntaxe

```python
from sklearn.model_selection import TimeSeriesSplit

tscv = TimeSeriesSplit(n_splits=5)

for train_idx, test_idx in tscv.split(X):
    X_train = X.iloc[train_idx]
    X_test = X.iloc[test_idx]
```

## Paramètres principaux

|Paramètre|Rôle|
|---|---|
|`n_splits=5`|Nombre de découpages|
|`test_size`|Nombre d'observations dans chaque test|
|`max_train_size`|Taille maximale de l'entraînement|
|`gap`|Nombre d'observations exclues entre train et test|

## Fonctionnement

Exemple avec trois découpages :
![[Pasted image 20261017114429.png]]
L'ensemble d'entraînement s'agrandit progressivement, tandis que les observations du test sont toujours postérieures à celles du train.

## Vigilances

- Les données doivent être triées chronologiquement avant le découpage.
    
- Ne pas mélanger les observations avec `shuffle`.
    
- Les observations doivent idéalement être espacées régulièrement pour comparer des périodes de test équivalentes.
    
- La méthode ne réalise pas automatiquement le prétraitement des données.
    
- `TimeSeriesSplit` sert à la validation croisée, pas nécessairement à définir le jeu de test final.
    

## Voir aussi

- [[05 Préparation et traitements spécialisés/00 Préprocessing transversal/4. Train test set/Train_test_split|train_test_split]]
    
- [[06 Modélisation ML/03 Validation et optimisation/Cross_validate|Validation croisée]]
    
- [[05 Préparation et traitements spécialisés/01 Séries temporelles/Séries temporelles]]