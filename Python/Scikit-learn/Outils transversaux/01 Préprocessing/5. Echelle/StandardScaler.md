# Standardiser 

## Quand l’utiliser ?

Préprocessing. On peut parler également de normalisation. 
Rendre les variables quantitatives comparables pour SVM, KNN, régressions pénalisées ou PCA.
![[Pasted image 20260930140427.png]]
## Principe

Soustraire la moyenne et diviser par l’écart-type appris sur l’entraînement, colonne par colonne.
On parle aussi de centrer / réduire. Cela permet de transformer en unités compatible avec distribution de moyenne 0 et écrat type de 1. 

> Cours : classification des vins par SVM. Cette fiche suit le scaler jusqu’à son utilisation par le modèle.

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

---

# Pattern

## Appliquer la normalisation
 
 Les distances ou la pénalisation dépendent des unités. `fit_transform` apprend les moyennes et écarts-types **sur le train**, puis le transforme. `transform` réutilise ces valeurs sur le test ; le test n’a pas à être exactement centré-réduit.

```python
from sklearn.preprocessing import StandardScaler
#format numpy
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

#format dataframe
X_train_scaled = pd.DataFrame(
    scaler.fit_transform(X_train),
    columns=X_train.columns,
    index=X_train.index
)
X_test_scaled = pd.DataFrame(
    scaler.transform(X_test),
    columns=X_test.columns,
    index=X_test.index
)
```

## Contrôler ce qui a été appris

Les moyennes et facteurs de division sont ceux du train. Les colonnes constantes ne deviennent pas de variance 1.

```python
print(pd.DataFrame({"moyenne_train": scaler.mean_, "facteur_echelle": scaler.scale_}, index=X.columns))
print(pd.DataFrame(X_train_modele, columns=X.columns).head())
print("Moyennes train transformé :", X_train_modele.mean(axis=0))
print("Moyennes test transformé :", X_test_modele.mean(axis=0))
```


## Paramètres utiles

- `with_mean=True` : centre les colonnes ; `with_std=True` : réduit leur dispersion.
- `mean_` et `scale_` : moyennes et facteurs d’échelle appris ; une colonne constante n’est pas divisée par zéro.
- `fit_transform` apprend puis transforme ; `transform` réutilise les statistiques.

## Interprétation

Les colonnes du train ont une moyenne proche de 0 et une variance proche de 1 si elles ne sont pas constantes. Le test n’a pas à avoir exactement ces statistiques.



## Points de vigilance

- Sur une matrice creuse, utiliser with_mean=False pour éviter de la densifier.
- Le scaler est sensible aux valeurs aberrantes ; il ne les supprime pas.
- Avec une validation croisée, le prétraitement doit être réappris dans chaque pli. Standardiser une fois tout le train avant la CV laisse les plis de validation influencer le prétraitement ; le test final reste séparé, mais le score de CV peut être optimiste. Les pipelines seront traités ultérieurement.
- Le cours montre aussi preprocessing.scale : sans objet appris conservé, ce raccourci est moins adapté pour appliquer la même transformation à un test.


## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de regression.md`.

- [preprocessing.StandardScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)
- [preprocessing.scale](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.scale.html)

```Python
#Code utilisé dans le cours pour montrer ce qui est une standardisation
X_train_scaled=preprocessing.scale(X_train)

print("Moyenne", X_train_scaled.mean(axis=0))

print("Ecart-type", X_train_scaled.std(axis=0))
```