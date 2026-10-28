# Indexer et modifier un tenseur PyTorch

## Syntaxe

```python
valeur = tensor[1, 3]
sous_matrice = tensor[1:4, 2:5]
tensor[1, 3] = 1
```

## Paramètres

Chaque position correspond à une dimension. Une tranche suit `début:fin:pas`, avec `fin` exclue.

## Vigilances & tips

Une modification en place d'un tenseur participant à un calcul de gradient peut perturber l'autodifférentiation.
