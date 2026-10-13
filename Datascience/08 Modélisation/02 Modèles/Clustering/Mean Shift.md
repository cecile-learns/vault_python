# Mean Shift

## Quand l’utiliser ?

Repérer automatiquement des groupes autour de **zones de forte densité**, sans fixer directement le nombre de clusters.

Exemples :
- segmentation de profils ou comportements ;
- regroupement de points géographiques ou de mesures proches ;
- réduction du nombre de couleurs d’une image.


## Principe

Les centres se déplacent vers des zones de densité élevée dans un voisinage contrôlé par bandwidth.

> Cours : réduire la palette de bird_small.png en regroupant les couleurs RGB.


---

# Pas à pas

## 1. Passer de l’image à une matrice de couleurs

Chaque ligne de X est un pixel, chaque colonne une composante couleur. Vérifier la présence de trois canaux ; la transparence éventuelle n’est pas une couleur RGB.

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.cluster import MeanShift, estimate_bandwidth


#exemple du cours
img = plt.imread("bird_small.png")
rgb = img[:, :, :3]
X = rgb.reshape(-1, 3)
```

## 2. Estimer le voisinage

quantile contrôle l’échelle du voisinage ; n_samples limite le coût de son estimation. La bande passante doit être positive.

```python
from sklearn.cluster import estimate_bandwidth
#Données tabulaire
bandwidth = estimate_bandwidth(
    X,
    quantile=0.1,
    n_samples=min(300, len(X)),
    random_state=42
)

#image
bandwidth = estimate_bandwidth(
    X,
    quantile=0.1,
    n_samples=min(300, len(X)),
    random_state=42
)
```

## 3. Entrainer Mean Shift

```Python
from sklearn.cluster import MeanShift

#Données tabulaires
modele = MeanShift(bandwidth=bandwidth)
modele.fit(X)

groupes = modele.labels_
centres = modele.cluster_centers_

#image
modele = MeanShift(bandwidth=bandwidth)
modele.fit(X)
groupes = modele.labels_
centres = modele.cluster_centers_
print("Nombre de couleurs représentatives :", len(centres))

```
Mean Shift détermine le nombre de centres selon les données et le voisinage ; on ne lui fournit pas n_clusters.

## 4. Exploiter les clusters



```python
#Données tabulaires
df["cluster"] = groupes
df.groupby("cluster").mean(numeric_only=True) 
"""
Questions :

- combien de clusters ?
- quelles variables distinguent les groupes ?
- les groupes ont-ils un sens métier ?
"""

#image : Remplacer chaque pixel par la couleur du centre de son cluster :
couleurs_reconstruites = centres[groupes]
image_reconstruite = couleurs_reconstruites.reshape(rgb.shape)

```


# Évaluer le résultat

Comparer d’abord l’information visuelle conservée et le nombre de couleurs. La taille d’un fichier compressé dépend aussi de l’encodage ; le nombre de centres ne suffit pas à la déduire.

```python
#Données tabulaires : 
from sklearn.metrics import silhouette_score

score = silhouette_score(X, groupes)
print(score)

#image
print("Nombre de couleurs représentatives :", len(centres))

#Afficher les images
# Redimensionnement de la matrice X
X_recovered = X_recovered.reshape(img.shape[0], img.shape[1], img.shape[2])
# Affichage de l'image originale et celle reconstruite
plt.figure()
plt.subplot(121)
plt.imshow(img)
plt.title('Image originale')
plt.subplot(122)
plt.imshow(X_recovered)
plt.title('Image reconstruite')
plt.show()
```

## Mesurer le rapport de tailles — optionnel

Ce bloc écrit **un nouveau fichier de résultat** dans le dossier de travail du notebook. Garder l’original intact.

```python
#Taux de compression
# Importation des packages nécessaires
import imageio
import os

# Enregistrement de l'image reconstruite
imageio.imwrite('bird_small_recovered_new.png', X_recovered)

# Taille en octets de l'image originale
original_size = os.stat('bird_small.png').st_size

# Taille en octets de l'image reconstruite
recovered_size = os.stat('bird_small_recovered_new.png').st_size

# Caclul et affichage du ratio de compression
compression_rate = round(recovered_size/original_size*100,2)
print('Ratio de compression : ', compression_rate, '%')

#Autre proposition du vault
from pathlib import Path

sortie = Path("bird_small_palette_reduite.png")
if sortie.exists():
    raise FileExistsError("Choisir un nom de sortie encore inutilisé.")
plt.imsave(sortie, image_reconstruite)
ratio = sortie.stat().st_size / Path("bird_small.png").stat().st_size
print("Taille finale / initiale :", ratio)
```

Un ratio de 0,6 représente une taille finale de 60 % de l’original, soit 40 % de réduction. Ce n’est pas une métrique de qualité visuelle.

## Paramètres utiles

- `bandwidth` : rayon du voisinage dans l’espace des variables ; vérifier que son estimation est strictement positive.
- `quantile` : contrôle l’échelle de distance estimée ; ce n’est pas un nombre de clusters.
- `n_samples` : nombre d’observations utilisées pour estimer la bande passante ; réduire ce nombre limite le coût.
- `random_state` : tirage reproductible du sous-échantillon.

## Interprétation


Le nombre de centres trouvés correspond au nombre de groupes détectés.

Une petite bande passante tend à créer davantage de groupes ; une bande passante plus grande fusionne davantage les zones proches.

Dans l’exemple image, ces groupes correspondent à des familles de couleurs.
Dans d’autres contextes, ils peuvent correspondre à des profils, des comportements ou des zones de densité dans l’espace des variables.


# Pattern minimal

Avec X et une bande passante déjà déterminés :

```python
modele = MeanShift(bandwidth=bandwidth).fit(X)
image_reconstruite = modele.cluster_centers_[modele.labels_].reshape(rgb.shape)
```



## Points de vigilance

- Dans l’exemple, X représente des couleurs RGB : les coordonnées spatiales des pixels ne sont pas incluses, donc le modèle ne recherche pas leur contiguïté dans l’image.
- Estimer bandwidth et entraîner MeanShift peut coûter cher.
- Réduire le nombre de couleurs ne garantit pas à lui seul un fichier plus petit ; cela dépend aussi de son encodage.


## Références

Source de formation : `01_RESSOURCES/Liora/Clustering.md`.

- [cluster.MeanShift](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.MeanShift.html)
- [cluster.estimate_bandwidth](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.estimate_bandwidth.html)

