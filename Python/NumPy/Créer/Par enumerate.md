
# Par enumerate

## Syntaxe

```python
import numpy as np

X=np.ones((10,4))

for i, ligne in enumerate (X):
    X[i,:]=ligne*i
print(X)
```

## Fonction

Crée un array NumPy.
