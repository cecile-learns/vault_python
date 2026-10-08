# opérations vectorisées

## Syntaxe

```python
resultat = tableau * 2 + 1

def normalisation_min_max(X):
    X_tilde = np.zeros((3, 2))
    
    V_min = np.min(X, axis=0)
    V_max = np.max(X, axis=0)
    V_ecart=V_max - V_min
    
    X_tilde=(X - V_min) / V_ecart
    
    return X_tilde

X = np.array([[24, 1.88],
              [18, 1.68],
              [14, 1.65]])

X_tilde = normalisation_min_max(X)

print(X_tilde)

[[1.         1.        ]
 [0.4        0.13043478]
 [0.         0.        ]]
```

## Fonction

Applique une opération élément par élément sans boucle Python.
broacasting : 
- les dimensions sont égales.
    
- une des dimensions vaut 1.
