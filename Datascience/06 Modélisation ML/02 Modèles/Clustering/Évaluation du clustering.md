# Évaluation du clustering

## Quand l’utiliser ?

Comparer plusieurs découpages sans labels de référence.

## Principe

Le coude cherche un compromis entre compacité et nombre de groupes. La silhouette compare proximité interne et séparation du groupe voisin.

> Cours : coude pour K-Means sur Ruspini, puis silhouette pour la CAH des fromages. Deux jeux distincts, sans comparer leurs scores entre eux.

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

---

# Pas à pas

## 1. Préparer les coordonnées Ruspini

La distorsion du code est une distance moyenne, pas le ratio SSW/(SSW+SSB) cité ailleurs dans le support.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans, AgglomerativeClustering
from sklearn.metrics import silhouette_score
from scipy.spatial.distance import cdist

X_ruspini = pd.read_csv("ruspini.csv", index_col="indices")
assert X_ruspini.notna().all().all()
```

## 2. Tracer le coude

Augmenter k réduit généralement la distance aux centres. Chercher un compromis, pas le minimum brut.

```python
valeurs_k = [2, 3, 4, 5, 6]
distorsions = []
for k in valeurs_k:
    modele = KMeans(n_clusters=k, n_init=10, random_state=42).fit(X_ruspini)
    distorsions.append(cdist(X_ruspini, modele.cluster_centers_).min(axis=1).mean())
plt.plot(valeurs_k, distorsions, "o-")
plt.xlabel("Nombre de groupes")
plt.ylabel("Distance moyenne au centre")
plt.show()
```

## 3. Comparer les silhouettes des fromages

La métrique sqeuclidean reprend le choix du cours. Conserver l’espace et la distance lorsqu’on compare plusieurs k.

```python
X_fromages = pd.read_csv("fromage.txt", sep="	", index_col=0)
assert X_fromages.notna().all().all()
valeurs_k_cah = list(range(2, min(15, len(X_fromages))))
silhouettes = []
for k in valeurs_k_cah:
    cah = AgglomerativeClustering(n_clusters=k).fit(X_fromages)
    silhouettes.append(silhouette_score(X_fromages, cah.labels_, metric="sqeuclidean"))
plt.plot(valeurs_k_cah, silhouettes, "o-")
plt.xlabel("Nombre de groupes")
plt.ylabel("Silhouette")
plt.show()
```

# Interprétation

Silhouette proche de 1 : groupes compacts et séparés ; proche de 0 : chevauchement ; négative : affectations discutables. Le calcul exige entre 2 et n−1 labels distincts. Un maximum ne suffit pas : examiner les effectifs, les profils et le sens métier. Les numéros des groupes ne sont pas des labels supervisés à comparer par accuracy.

## Paramètres utiles

- `valeurs_k` : nombres de groupes ; pour la silhouette, entre 2 et n_observations − 1, avec effectivement ce nombre de labels distincts.
- `cdist` (SciPy) : distances de chaque observation à chaque centre.
- `np.min(..., axis=1).mean()` : distance moyenne au centre le plus proche, telle que calculée dans le cours.
- `metric="sqeuclidean"` : distance euclidienne au carré employée dans la silhouette du cours ; `"euclidean"` est une autre convention courante.

## Interprétation

La distorsion calculée ici n’est ni l’inertie ni le ratio SSW/(SSW+SSB). La silhouette va de −1 à 1 : près de 1, séparation favorable ; près de 0, chevauchement ; négative, affectation potentiellement discutable.

# Pattern minimal

Pour un regroupement déjà calculé, avec X dans l’espace utilisé pour les distances :

```python
score = silhouette_score(X, groupes, metric="euclidean")
```



## Points de vigilance

- Ne pas comparer des silhouettes calculées dans des espaces ou avec des distances différentes.
- Le code reprend séparément les deux séquences KMeans/coude et CAH/silhouette du cours.
- Les numéros de clusters ne sont pas des classes réelles à comparer directement par accuracy.


## Références

Source de formation : `01_RESSOURCES/Liora/Clustering.md`.

- [metrics.silhouette_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.silhouette_score.html)
- [cluster.KMeans](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)
- [cluster.AgglomerativeClustering](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.AgglomerativeClustering.html)

