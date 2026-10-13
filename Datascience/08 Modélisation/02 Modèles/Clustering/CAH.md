# Classification ascendante hiérarchique

## Quand l’utiliser ?

Construire des groupes emboîtés et choisir un niveau de regroupement. Exemple : profils nutritionnels de fromages.

## Principe

AgglomerativeClustering fusionne progressivement les groupes selon une règle de liaison. Ward cherche des fusions limitant l’augmentation de variance interne.



---

# Pas à pas

## 1. Charger les mesures

Le nom du fromage est un index. Le cours emploie les mesures brutes ; des unités hétérogènes peuvent dominer la distance. Une standardisation est une autre analyse, dont le dendrogramme et les seuils changeraient.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.cluster import AgglomerativeClustering
from scipy.cluster.hierarchy import linkage, dendrogram
from sklearn.metrics import silhouette_score

df = pd.read_csv("fromage.txt", sep="	", index_col=0)
assert df.notna().all().all()
X = df.copy()
```

## 2. Examiner le dendrogramme

SciPy construit la hiérarchie et son tracé. Ward utilise la distance euclidienne. La hauteur des fusions aide à proposer un découpage.

```python
liaisons = linkage(X, method="ward", metric="euclidean")
plt.figure(figsize=(14, 6))
dendrogram(liaisons, labels=list(df.index), leaf_rotation=90, color_threshold=0)
plt.ylabel("Distance de fusion")
plt.show()
```

## 3. Comparer plusieurs découpages

Conserver les mêmes données et la même distance de silhouette pour comparer les nombres de groupes. Le cours emploie sqeuclidean.

```python
valeurs_k = list(range(2, min(15, len(X))))
scores = []
for k in valeurs_k:
    essai = AgglomerativeClustering(n_clusters=k, linkage="ward").fit(X)
    scores.append(silhouette_score(X, essai.labels_, metric="sqeuclidean"))
plt.plot(valeurs_k, scores, "o-")
plt.xlabel("Nombre de groupes")
plt.ylabel("Silhouette")
plt.show()
```

## 4. Former et lire les groupes

Quatre correspond au premier essai du cours ; sa comparaison par silhouette conduit à examiner aussi deux groupes. Le choix doit rester justifiable dans le contexte.

```python
modele = AgglomerativeClustering(n_clusters=4, linkage="ward", metric="euclidean")
modele.fit(X)
groupes = modele.labels_
print(df.assign(groupe=groupes).sort_values("groupe"))
print(df.assign(groupe=groupes).groupby("groupe").mean())
```

# Évaluation

Confronter la silhouette, les hauteurs de fusion et les profils nutritionnels. Les numéros de groupes n’ont pas d’ordre. Cet estimateur n’a pas de predict pour de nouveaux fromages ; il ne s’agit pas d’un classifieur supervisé.
![[Pasted image 20261006102732.png]]
## Paramètres utiles

- `n_clusters` : nombre final de groupes.
- `linkage` : `"ward"`, `"complete"`, `"average"` ou `"single"` ; détermine la règle de fusion.
- `metric` : distance ; Ward impose `"euclidean"`.
- `distance_threshold` : seuil de fusion alternatif ; exige `n_clusters=None` et une construction complète de l’arbre.

## Interprétation

`labels_` attribue un groupe à chaque ligne. La hiérarchie n’est pas une classification supervisée : aucune cible n’est apprise.

# Pattern minimal

Avec X préparé :

```python
modele = AgglomerativeClustering(n_clusters=4).fit(X)
groupes = modele.labels_
```



## Points de vigilance

- Pas de méthode predict pour affecter directement de nouveaux points avec cet estimateur.
- Les unités et la règle de liaison peuvent changer fortement les groupes.
- La mémoire et le calcul peuvent devenir importants sur beaucoup d’observations.


## Références

Source de formation : `01_RESSOURCES/Liora/Clustering.md`.

- [cluster.AgglomerativeClustering](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.AgglomerativeClustering.html)
- [linkage](https://docs.scipy.org/doc/scipy/reference/generated/scipy.cluster.hierarchy.linkage.html) et [dendrogram](https://docs.scipy.org/doc/scipy/reference/generated/scipy.cluster.hierarchy.dendrogram.html) — SciPy.
