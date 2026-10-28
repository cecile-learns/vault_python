# ACP — Analyse en composantes principales

## Quand l’utiliser ?

Réduire le nombre de dimensions d’un jeu de données numériques en créant de **nouvelles variables synthétiques** qui conservent le maximum d’information possible.

Usages principaux :

- visualiser des données en 2D ou 3D ;
- réduire le nombre de variables avant un modèle ;
- résumer des variables fortement corrélées. Réduction de dimension

## Principe

L’ACP construit de nouveaux axes appelés **composantes principales**.

```
variables d'origine
→ combinaisons linéaires
→ nouvelles composantes
```

Les composantes :

- sont décorrélées entre elles ;
- sont classées de la plus informative à la moins informative ;
- captent successivement le maximum de variance possible. Réduction de dimension

## Préparer les données

### Retirer la cible

La PCA travaille sur les variables explicatives, pas sur `y`.

```python
target = df["label"]
data = df.drop(columns="label")
```

### Vérifier les échelles

Si les variables sont dans des unités ou échelles différentes :

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

Si toutes les variables sont déjà comparables, comme les pixels Fashion-MNIST, la standardisation n’est pas forcément nécessaire. 
## Appliquer une ACP

### Fixer directement le nombre de composantes

```python
from sklearn.decomposition import PCA

pca = PCA(n_components=2)
X_pca = pca.fit_transform(X)

#option du cours
print("La part de variance expliquée pour chaque axe", pca.explained_variance_ratio_)

print("La part de variance expliquée cumulée par les deux axes est", round(pca.explained_variance_ratio_.sum(),2))
```

Résultat :

```
X : n observations × p variables
↓
ACP
↓
X_pca : n observations × 2 composantes
```

### Rechercher une proportion de variance

```Python
pca=PCA()
pca.fit(X)

plt.figure()
plt.xlim(0,100)
plt.plot(pca.explained_variance_ratio_);

plt.figure()
plt.xlim(0,100)
plt.xlabel('Nombre de composantes')
plt.ylabel('Part de variance expliquée')
plt.axhline(y = 0.9, color ='r', linestyle = '--')
plt.plot(pca.explained_variance_ratio_.cumsum());
```
### Conserver une proportion de variance

```python
pca = PCA(n_components=0.9)
X_pca = pca.fit_transform(X)
```

Ici, PCA conserve automatiquement le nombre minimal de composantes nécessaires pour atteindre environ 90 % de variance expliquée. 

## Choisir le nombre de composantes

### Variance expliquée par chaque axe

```python
pca.explained_variance_ratio_
```

Chaque valeur indique la part de variance expliquée par une composante.

### Variance cumulée

```python
pca.explained_variance_ratio_.cumsum()
```

Deux approches vues dans le cours :

```
méthode du coude
→ repérer quand ajouter des composantes apporte peu

variance cumulée
→ choisir un seuil, par exemple 90 %
```

Réduction de dimension

## Train / test

Si l’ACP fait partie d’un workflow de ML :

```python
pca = PCA(n_components=0.9)X_train_pca = pca.fit_transform(X_train)X_test_pca = pca.transform(X_test)
```

Règle :

```
train
→ apprend les axes + projection

test
→ projection sur les mêmes axes
```

Il ne faut pas recalculer une nouvelle PCA sur le test. Réduction de dimension

## Ce que `fit` apprend

`fit` apprend :

- les directions des composantes principales ;
- leur importance en termes de variance ;
- le nombre de composantes retenues si celui-ci dépend d’un seuil.

Attributs utiles :

```python
pca.components_pca.explained_variance_ratio_pca.n_components_
```

## Ce que retourne l’ACP

`fit_transform()` retourne un `ndarray`.

```
lignes
→ mêmes observations qu'au départ

colonnes
→ nouvelles composantes principales
```

Les composantes **ne sont pas des variables sélectionnées parmi les anciennes**. Ce sont de nouvelles variables construites à partir d’elles.

## Pattern minimal

```python
from sklearn.decomposition import PCApca = PCA(n_components=0.9)X_train_pca = pca.fit_transform(X_train)X_test_pca = pca.transform(X_test)
```

## Points de vigilance

- Ne pas inclure la cible dans l’ACP.
- Standardiser lorsque les variables sont sur des échelles différentes.
- `90 % de variance conservée` ne signifie pas `90 % de performance prédictive`.
- L’ACP capte des relations **linéaires**.
- Les nouvelles composantes perdent en partie l’interprétation directe des variables d’origine.

## À retenir

```
ACP
= variables d'origine
→ nouveaux axes décorrélés
→ maximum de variance conservée
→ moins de dimensions
```