# t-SNE

## Quand l’utiliser ?

Visualiser des voisinages en deux dimensions. Exemple : exploration de Fashion-MNIST.

## Principe

t-SNE rapproche des observations ayant des voisinages similaires ; son objectif est surtout exploratoire.

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
from sklearn.manifold import TSNE

assert len(X) > 30
reducteur = TSNE(n_components=2, method="barnes_hut", perplexity=30, random_state=42)
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

Pour t-SNE, les distances entre amas et leurs tailles sont particulièrement délicates à interpréter. sklearn.TSNE n’a pas de transform : on ne peut pas appliquer simplement cet objet à un nouveau test.

## Paramètres utiles

- `n_components=2` : deux coordonnées pour tracer.
- `method="barnes_hut"` : approximation efficace, réservée à une sortie de dimension inférieure à 4.
- `perplexity` : échelle du voisinage ; doit être strictement inférieure au nombre d’observations.
- `random_state` : reproductibilité de l’optimisation ; `max_iter` : budget d’itérations dans l’API actuelle.

## Interprétation

`coordonnees` contient une ligne par observation et une colonne par dimension de sortie. Les axes n’ont pas de signification métier directe.

# Pattern minimal

Sur X déjà prêt pour l’exploration :

```python
reducteur = TSNE(n_components=2, method="barnes_hut", perplexity=30, random_state=42)
coordonnees = reducteur.fit_transform(X)
```



## Points de vigilance

- La taille des amas et les distances entre amas ne se lisent pas comme les distances originales ; plusieurs réglages et graines peuvent produire des vues différentes.
- Une projection destinée à un modèle prédictif doit être apprise sur le train seul ; une visualisation de toutes les données reste exploratoire.
- TSNE ne fournit pas transform pour de nouvelles observations ; ne pas ajuster séparément train et test pour ensuite comparer leurs coordonnées.


## Références

Source de formation : `01_RESSOURCES/Liora/Réduction de dimension.md`.

- [manifold.TSNE](https://scikit-learn.org/stable/modules/generated/sklearn.manifold.TSNE.html)

