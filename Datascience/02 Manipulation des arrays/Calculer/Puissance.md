

# Puissance **2

## Syntaxe

```python
A=np.array([[1,-1],[-1,1]])

def puissanceA(n):
    A_n=np.array([[1,0],[0,1]])
    for i in range(n):
        A_n=A_n.dot(A)

    return A_n


print("A**1: \n", puissanceA(1), "\n")
print("A**2: \n", puissanceA(2), "\n")
print("A**3: \n", puissanceA(3), "\n")
print("A**4: \n", puissanceA(4), "\n")
```

## Fonction

Exemple basé sur un exercice ouù on utilise dot. attention ici pour la matrice de départ il faut la matrice d'identité



