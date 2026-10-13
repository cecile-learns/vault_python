# random — tirages aléatoires

Module de la bibliothèque standard : nécessite `import random`. Rangé ici comme aide de recherche ; ce n’est pas une fonction intégrée telle que `len()`.

## Syntaxe — 40 entiers entre 0 et 10

```python
import random

valeurs = [random.randint(0, 10) for _ in range(40)]
```

## Paramètres

- `randint(0, 10)` : un entier entre les deux bornes incluses.
- `range(40)` : répète le tirage 40 fois ; `_` est le compteur inutilisé.
- Les crochets construisent une liste ; les valeurs peuvent se répéter.

## Mémo — possibilités courantes

Après `import random`, utiliser la ligne correspondant au besoin.

| Besoin | Syntaxe |
|---|---|
| Un entier, 0 et 10 inclus | `entier = random.randint(0, 10)` |
| Un entier, 0 inclus et 10 exclu | `entier = random.randrange(0, 10)` |
| Un flottant, 0 inclus et 1 exclu | `decimal = random.random()` |
| Un flottant entre 0 et 10 | `decimal = random.uniform(0, 10)` |
| 40 flottants entre 0 et 10 | `valeurs = [random.uniform(0, 10) for _ in range(40)]` |
| Un élément d’une liste non vide | `element = random.choice(liste_source)` |
| Plusieurs tirages avec remise | `selection = random.choices(liste_source, k=5)` |
| Plusieurs tirages sans remise | `selection = random.sample(liste_source, k=5)` |
| Mélanger une liste sur place | `random.shuffle(liste_source)` |

`liste_source` est la liste dans laquelle tirer ou à mélanger ; `k` est le nombre d’éléments tirés. Avec remise : une même position peut être tirée plusieurs fois. Sans remise : chaque position est tirée au plus une fois, mais une liste contenant des doublons peut fournir des valeurs identiques.

## Écriture rencontrée dans le cours — NumPy

```python
import numpy as np

tableau = np.random.choice(11, 40)
valeurs = tableau.tolist()  # Si une liste Python est nécessaire

# Autre écriture pour le même besoin (borne supérieure exclue) :
# tableau = np.random.randint(0, 11, size=40)
```

`11` désigne les entiers de 0 à 10 ; `40` est le nombre de tirages. La remise est activée par défaut (`replace=True`) : les répétitions sont possibles. Sans probabilités fournies, chaque entier a la même probabilité.

`np.random.choice` renvoie ici un tableau NumPy ; la compréhension avec `random.randint` renvoie une liste Python. Les deux répondent au même besoin, sans produire nécessairement les mêmes tirages. `random` et `np.random` sont deux modules distincts : `random.seed` ne règle pas le générateur NumPy.

## Fonction

Produit des tirages pseudo-aléatoires ou mélange une liste. `random.random()` est une fonction du module `random`, dédiée aux flottants entre 0 inclus et 1 exclu.

## Vigilances & tips

- Il existe seulement 11 entiers de 0 à 10 : impossible d’en obtenir 40 tous différents.
- `sample` exige `k` inférieur ou égal à la taille de la population.
- `shuffle` modifie la liste et renvoie `None` : ne pas écrire `liste_source = random.shuffle(liste_source)`.
- Avec `uniform`, l’inclusion de la borne supérieure dépend des arrondis des flottants.
- Pour reproduire une séquence dans le même environnement, appeler `random.seed(42)` une fois avant les tirages. `42` est un choix arbitraire ; ne pas réinitialiser dans la boucle.

## Documentation officielle

[Python — module random](https://docs.python.org/3/library/random.html)

[NumPy — random.choice et exemple équivalent avec randint](https://numpy.org/doc/stable/reference/random/generated/numpy.random.choice.html)

Source : besoin de créer une liste aléatoire et mémo des usages courants demandé dans cette discussion.
