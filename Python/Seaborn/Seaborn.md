# Seaborn

Choisir d’abord la situation de données, puis l’intention du graphique. Seaborn s’appuie sur Matplotlib et ajoute des traitements statistiques, des regroupements et des thèmes prêts à l’emploi.

- [[Seaborn/01 Une variable quantitative/Une variable quantitative|Une variable quantitative]]
- [[Seaborn/02 Une variable qualitative/Une variable qualitative|Une variable qualitative]]
- [[Seaborn/03 Deux variables quantitatives ou plus/Deux variables quantitatives ou plus|Deux variables quantitatives ou plus]]
- [[Seaborn/04 Une quantitative et une qualitative/Une quantitative et une qualitative|Une quantitative et une qualitative]]
- [[Seaborn/06 Syntaxe et ajustements/Syntaxe et ajustements|Syntaxe et ajustements]]


```python

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns

sns.set() # pour modifier le thème

df = pd.read_csv('', index_col= 0)
df.head()
```

