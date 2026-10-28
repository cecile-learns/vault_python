# Régression logistique

## Quand l’utiliser ?

Prédire une classe binaire ou multiclasse ; obtenir des probabilités de classe. Exemple du cours : admission d’un candidat.

## Principe

Un score linéaire est converti en probabilité par la fonction logistique en binaire ; en multiclasse, la formulation multinomiale utilise une normalisation softmax. Malgré son nom, il s’agit ici de classification.

> Exemple du cours : admissions.csv — prestige, GRE et GPA pour prédire admit (0 ou 1).

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

---

# Pas à pas

## 1. Préparer et discrétiser les variables

La discrétisation est le choix de cet exercice, **pas une obligation** de la régression logistique. Garder les colonnes d’origine. Les bornes sont fixées ici à l’avance ; des bornes estimées sur les données devraient être apprises sur le train.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("admissions.csv")
df = df.dropna(subset=["admit", "prestige", "gre", "gpa"]).copy()
df["gre_d"] = pd.cut(df["gre"], bins=[200, 450, 550, 620, 800],
                     labels=["mauvais", "moyen", "moyen+", "bon"], include_lowest=True)
df["gpa_d"] = pd.cut(df["gpa"], bins=[2, 2.8, 3.2, 3.6, 4],
                     labels=["mauvais", "moyen", "moyen+", "bon"], include_lowest=True)
print(df[["gre", "gre_d", "gpa", "gpa_d"]].head())
print(df[["gre_d", "gpa_d"]].isna().sum())
# Arrêter et examiner les bornes si une valeur n'a pas été classée.
assert df[["gre_d", "gpa_d"]].notna().all().all()
```

`include_lowest=True` inclut la plus petite borne (200 pour GRE, 2 pour GPA). Les autres intervalles sont fermés à droite par défaut. Une valeur hors bornes devient manquante : ne pas l’ignorer.

## 2. Définir X et y

X contient uniquement les variables retenues, y la cible. Cela évite de garder à la fois les versions continues et discrétisées par erreur.

```python
X = df[["prestige", "gre_d", "gpa_d"]]
y = df["admit"]
print(X.shape, y.shape)
```

Alternative : `X = df.drop("admit", axis=1)` convient seulement si **toutes les autres colonnes** sont voulues comme variables explicatives. Ici, la sélection explicite conserve ta stratégie.

## 3. Séparer train et test

`X` est une matrice 2D, `y` un vecteur 1D. Les quatre sorties restent alignées. Le train sert à apprendre ; le test reste à l’écart du choix des réglages. En classification, on peut ajouter `stratify=y` si les effectifs le permettent ; le découpage ci-dessous reprend celui du cours.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=66
)
print(X_train.shape, X_test.shape, y_train.shape, y_test.shape)
```

## 4. Encoder les catégories (si modèle ML de type linéaire)

L’encodeur apprend les modalités du train. Sur le test, on utilise seulement transform pour conserver les mêmes colonnes.

```python
from sklearn.preprocessing import OneHotEncoder

enc = OneHotEncoder(handle_unknown="ignore")
X_train_enc = enc.fit_transform(X_train)
X_test_enc = enc.transform(X_test)
apercu = pd.DataFrame(X_train_enc[:5].toarray(), columns=enc.get_feature_names_out())
print(apercu)
```

`get_feature_names_out()` nomme les colonnes indicatrices. `.toarray()` convient ici à l’aperçu de cinq lignes puisque la sortie est creuse par défaut ; éviter de densifier une grande matrice.
*Dans la démonstration du cours, il est introduit la dichotomisation `OneHot Encoding`. Fiche sur le sujet en complément
## 5. Instancier et entraîner

C petit → régularisation forte ; C grand → régularisation faible. `max_iter` limite les itérations, sans garantir à lui seul la convergence.

```python
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression(C=1.0, max_iter=1000)
clf.fit(X_train_enc, y_train)
```

## 6. Prédire les classes

Le modèle utilise la représentation encodée du test, pas le tableau catégoriel brut.

```python
y_pred = clf.predict(X_test_enc)
```

# Évaluer le modèle

## Matrice de confusion

Lignes = classes réelles ; colonnes = classes prédites. La diagonale contient les bonnes classifications.

```python
from sklearn.metrics import confusion_matrix, classification_report

cm = confusion_matrix(y_test, y_pred, labels=clf.classes_)
print(pd.DataFrame(cm, index=clf.classes_, columns=clf.classes_))
# Alternative Pandas : mêmes observations dans le même ordre.
print(pd.crosstab(y_test, y_pred, rownames=["Classe réelle"], colnames=["Classe prédite"]))
```

## Accuracy et rapport par classe

Accuracy = proportion totale de bonnes réponses. La précision répond à « parmi les positifs prédits, combien sont corrects ? » ; le rappel à « parmi les positifs réels, combien sont retrouvés ? ». F1 combine précision et rappel ; support donne l’effectif réel de chaque classe.

```python
accuracy = clf.score(X_test_enc, y_test)
print("Accuracy :", accuracy)
print(classification_report(y_test, y_pred, zero_division=0))
```

Une bonne accuracy peut masquer un mauvais rappel de la classe minoritaire. `zero_division=0` fixe à zéro une métrique indéfinie ; regarder la classe concernée et la cause.

# Probabilités et seuil de décision

## Obtenir et lire les probabilités

La probabilité est une sortie du modèle. Le seuil transforme cette sortie en décision ; ce n’est pas une métrique.

```python
probs = clf.predict_proba(X_test_enc)
print(clf.classes_)
assert np.array_equal(clf.classes_, [0, 1])
proba_positive = probs[:, 1]
```

Après ce contrôle, `probs[:, 0]` concerne le label 0 et `probs[:, 1]` le label 1. Cette correspondance ne doit pas être supposée pour d’autres labels. En binaire, predict applique une décision correspondant au seuil usuel de 0,5 ; le seuil illustratif suivant est différent.

## Illustrer un seuil à 0,4

Le seuil est fixé ici pour reproduire l’exercice. Si on veut **le choisir**, utiliser une validation interne au train et conserver le test final intact.

```python
y_preds = np.where(proba_positive > 0.4, 1, 0)
cm_seuil = pd.crosstab(y_test, y_preds, rownames=["Classe réelle"], colnames=["Classe prédite"])
print(cm_seuil)
```

Un seuil plus bas donne davantage de positifs : le rappel peut augmenter, avec davantage de faux positifs. Le choix dépend du coût métier des erreurs.

# ROC / AUC

La ROC binaire met le taux de vrais positifs en regard du taux de faux positifs quand le seuil varie. Elle utilise **les probabilités**, pas y_pred. AUC proche de 1 : bonne discrimination ; proche de 0,5 : classement proche de l’aléatoire.

```python
from sklearn.metrics import roc_curve, auc

fpr, tpr, seuils = roc_curve(y_test, proba_positive, pos_label=1)
roc_auc = auc(fpr, tpr)
plt.plot(fpr, tpr, label=f"Régression logistique — AUC = {roc_auc:.2f}")
plt.plot([0, 1], [0, 1], linestyle="--", color="gray")
plt.xlabel("Taux de faux positifs")
plt.ylabel("Taux de vrais positifs")
plt.legend()
plt.show()
```

Le test doit contenir les deux classes. Ce code ROC ne se transpose pas directement à une classification multiclasse.

## Paramètres utiles

- `C` : inverse de la force de régularisation ; plus petit = pénalisation plus forte.
- `max_iter` : limite d’itérations d’optimisation ; augmenter ne remplace pas un prétraitement adapté.
- `fit(X_train, y_train)` apprend les paramètres ; `predict` retourne les classes ; `predict_proba` retourne une colonne par classe.
- `classes_` donne l’ordre des colonnes de probabilités.

## Interprétation

En binaire, vérifier quelle colonne correspond à la classe positive avant de changer le seuil. Un seuil plus bas favorise généralement le rappel au prix de davantage de faux positifs. Les probabilités restent des estimations, pas des certitudes.

# Pattern minimal

Avec les matrices **déjà encodées** :

```python
clf = LogisticRegression(C=1.0, max_iter=1000)
clf.fit(X_train_enc, y_train)
y_pred = clf.predict(X_test_enc)
probs = clf.predict_proba(X_test_enc)
```



## Points de vigilance

- Encoder les variables catégorielles. L’exemple admissions utilise OneHotEncoder après discrétisation.
- Mettre les variables quantitatives à une échelle comparable si nécessaire.
- Choisir le seuil sur un jeu de validation, jamais en optimisant sur le test final.
- Ne pas ignorer un avertissement de convergence ; vérifier les données et les réglages.


## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de classification.md`.

- [linear_model.LogisticRegression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)
- [ROC](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_curve.html) et [AUC](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.auc.html).

![[Pasted image 20260930115222.png]]
