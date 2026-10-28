# array

## Syntaxe

```python
tableau = np.array([[1, 2], [3, 4]])

#création d'une matrice 0 à 5 pour chaque ligne
matrice = np.zeros((6, 6))

for colonne in range(6):
    matrice[:, colonne] = colonne


X = np.array([i/100 for i in range(100)])
#imprime une seule ligne avec les pourcentage (mais sans pourcent)
```

## Fonction

Crée un array NumPy.

```python
tableau =
import numpy as np

tableau = np.random.choice(11, 40)
valeurs = tableau.tolist()  # Si une liste Python est nécessaire

# Autre écriture pour le même besoin (borne supérieure exclue) :
# tableau = np.random.randint(0, 11, size=40)
```