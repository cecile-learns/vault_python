# K-Means

## Quand l’utiliser ?

Former un nombre fixé de groupes compacts. Exemple du cours : données Ruspini.

## Principe

KMeans alterne affectation au centre le plus proche et recalcul des centres, en réduisant l’inertie intra-cluster.

> Cours : coordonnées du jeu ruspini.csv. On regroupe les observations sans cible connue.

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

---

# Pas à pas

## 1. Charger les coordonnées

L’index identifie les points ; il ne devient pas une variable de distance. Les coordonnées du cours sont gardées dans leur échelle commune.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
from scipy.spatial.distance import cdist

df = pd.read_csv("ruspini.csv", index_col="indices")
print(df.head())

clfassert df.notna().all().all()
X = df.copy()
```

## 2. Essayer plusieurs nombres de groupes

La distance moyenne au centre le plus proche reprend le calcul du cours. Elle diffère de l’inertie (distances au carré).

```python
valeurs_k = [2, 3, 4, 5, 6]
distorsions = []
for k in valeurs_k:
    essai = KMeans(n_clusters=k, n_init=10, random_state=42).fit(X)
    distances = cdist(X, essai.cluster_centers_, metric="euclidean")
    distorsions.append(distances.min(axis=1).mean())
plt.plot(valeurs_k, distorsions, "o-")
plt.xlabel("Nombre de groupes")
plt.ylabel("Distance moyenne au centre le plus proche")
plt.show()
```

## 3. Ajuster le découpage retenu

Le cours retient quatre groupes pour Ruspini ; ne pas réutiliser ce nombre automatiquement ailleurs.

```python
modele = KMeans(n_clusters=4, n_init=10, random_state=42)
modele.fit(X)
groupes = modele.labels_
centres = modele.cluster_centers_
print("Inertie :", modele.inertia_)
```

# Évaluer et interpréter les groupes

```python
from sklearn.metrics import silhouette_score

plt.scatter(X.iloc[:, 0], X.iloc[:, 1], c=groupes, cmap="tab10")
plt.scatter(centres[:, 0], centres[:, 1], marker="x", color="black", s=120)
plt.show()
print("Silhouette :", silhouette_score(X, groupes))
profils = df.assign(groupe=groupes).groupby("groupe").mean()
print(profils)
```

La silhouette exige entre 2 et n−1 groupes effectivement présents. Une silhouette élevée favorise compacité et séparation ; elle ne valide pas à elle seule une segmentation métier. Sur d’autres variables, discuter leur échelle avant tout calcul de distance.

## Paramètres utiles

- `n_clusters` : nombre de groupes à choisir ; 4 vient de l’exemple Ruspini, ce n’est pas une valeur générale.
- `n_init=10` : dix initialisations, garde celle de plus faible inertie ; valeur explicite pour éviter les changements de défaut entre versions.
- `random_state` : reproductibilité ; `max_iter` : nombre maximal d’itérations par initialisation.
- `labels_` : un numéro de groupe par ligne ; `cluster_centers_` : coordonnées des centres.

## Interprétation

Les numéros de groupes n’ont ni ordre ni sens métier intrinsèque. `inertia_` est la somme des distances euclidiennes au carré aux centres assignés.

# Pattern minimal

Sur une matrice X déjà préparée :

```python
modele = KMeans(n_clusters=4, n_init=10, random_state=42).fit(X)
groupes = modele.labels_
centres = modele.cluster_centers_
```



## Points de vigilance

- Sensible aux unités, aux valeurs extrêmes et aux initialisations.
- Convient moins aux groupes allongés, de densités très différentes ou non convexes.
- L’inertie diminue quand k augmente : la minimiser seule ne choisit pas un k utile.


## Références

Source de formation : `01_RESSOURCES/Liora/Clustering.md`.

- [cluster.KMeans](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)

