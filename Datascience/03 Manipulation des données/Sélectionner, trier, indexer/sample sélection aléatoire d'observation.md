# `sample()`

## Syntaxe

```python
df_echantillon = df.sample(n=10, random_state=42)
```

Avec une proportion :

```python
df_echantillon = df.sample(frac=0.20, random_state=42)
```

Avec remise :

```python
df_echantillon = df.sample(n=10, replace=True, random_state=42)
```

## Paramètres

- `n=10` : nombre de lignes à tirer aléatoirement.
- `frac=0.20` : proportion des lignes à tirer ; `0.20` correspond à 20 % du DataFrame.
- `random_state=42` : fixe le tirage pour obtenir le même échantillon à chaque exécution. `42` est arbitraire : n'importe quel entier peut servir de graine.
- `replace=True` : tirage **avec remise** ; une même ligne peut être sélectionnée plusieurs fois. Par défaut, `replace=False`.

## Fonction

`sample()` prélève aléatoirement des lignes dans un `DataFrame` ou des éléments dans une `Series`. Il retourne un nouvel objet contenant l'échantillon sélectionné.

## Vigilances & tips

Utiliser soit `n` pour demander un **nombre**, soit `frac` pour demander une **proportion** : on ne les utilise normalement pas ensemble.

Sans `random_state`, l'échantillon peut changer à chaque exécution.

Par défaut, le tirage est **sans remise** : une ligne ne peut apparaître qu'une fois dans l'échantillon.

**S'utilise souvent avec :** `head()`, `train_test_split()`
