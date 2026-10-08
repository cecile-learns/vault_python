# Isomap

## Quand l’utiliser ?

Déplier une structure non linéaire ; exemple Swiss roll du cours.

## Principe

Isomap cherche à préserver les distances géodésiques estimées à travers un graphe de voisins.

> Cours : projection exploratoire des 2 000 premières observations de Fashion-MNIST.

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

---

# Pas à pas

## 1. Préparer les observations à projeter

Les classes servent seulement à colorer le graphique. Les pixels partagent une même échelle ; un autre jeu peut nécessiter une standardisation.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("fashion_mnist_small.csv").iloc[:2000].copy()
X = df.drop(columns=["label"])
y_couleur = df["label"]
assert X.notna().all().all()
print(X.shape)
```

## 2. Calculer les coordonnées

Il n’y a pas de cible à apprendre pour cette projection. Le calcul peut être long ; commencer par un sous-échantillon permet de contrôler le déroulé.

```python
from sklearn.manifold import Isomap

assert len(X) > 50
reducteur = Isomap(n_neighbors=50, n_components=2)
coordonnees = reducteur.fit_transform(X)
print(coordonnees.shape)
```

## 3. Visualiser

Une ligne de coordonnées correspond à une observation de X. L’orientation des axes n’a pas de sens métier direct.

```python
nuage = plt.scatter(coordonnees[:, 0], coordonnees[:, 1], c=y_couleur, cmap="tab10", s=8, alpha=0.7)
plt.xlabel("Dimension 1")
plt.ylabel("Dimension 2")
plt.colorbar(nuage, label="Classe réelle, utilisée seulement pour la couleur")
plt.show()
```

# Évaluer la représentation

Examiner si des voisinages cohérents apparaissent, si les classes connues se mélangent et si la représentation reste semblable avec des réglages proches. **Ne pas interpréter le graphique comme une accuracy.** Réduire à deux dimensions perd de l’information ; une séparation apparente ne prouve pas une généralisation.

Pour un usage prédictif ultérieur, repartir d’un découpage train/test : apprendre le réducteur sur train, utiliser transform sur test, puis entraîner et évaluer le modèle final. Ne pas apprendre deux projections indépendantes.

## Paramètres utiles

- `n_neighbors` : taille du voisinage local, inférieure au nombre d’observations.
- `n_components` : nombre de coordonnées de sortie.

## Interprétation

`coordonnees` contient une ligne par observation et une colonne par dimension de sortie. Les axes n’ont pas de signification métier directe.

# Pattern minimal

Sur X déjà prêt pour l’exploration :

```python
reducteur = Isomap(n_neighbors=50, n_components=2)
coordonnees = reducteur.fit_transform(X)
```



## Points de vigilance

- Un voisinage trop petit peut déconnecter le graphe ; trop grand peut créer des raccourcis entre zones éloignées.
- Une projection destinée à un modèle prédictif doit être apprise sur le train seul ; une visualisation de toutes les données reste exploratoire.
- Pour de nouvelles observations, utiliser transform du réducteur appris ; ne pas réapprendre indépendamment un autre espace.


## Références

Source de formation : `01_RESSOURCES/Liora/Réduction de dimension.md`.

- [manifold.Isomap](https://scikit-learn.org/stable/modules/generated/sklearn.manifold.Isomap.html)

