# Évaluation des classifieurs


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
accuracy = clf.score(X_test_modele, y_test)
print("Accuracy :", accuracy)
print(classification_report(y_test, y_pred, zero_division=0))
```

Une bonne accuracy peut masquer un mauvais rappel de la classe minoritaire. `zero_division=0` fixe à zéro une métrique indéfinie ; regarder la classe concernée et la cause.

## Lire VP, VN, FP, FN en binaire

En imposant l’ordre [0, 1] et en définissant 1 comme positif, la matrice devient [VN, FP], [FN, VP]

```python
assert np.array_equal(clf.classes_, [0, 1])
vn, fp, fn, vp = confusion_matrix(y_test, y_pred, labels=[0, 1]).ravel()
print({"VN": vn, "FP": fp, "FN": fn, "VP": vp})
```

## F1 par classe ou moyenne

Macro donne le même poids à chaque classe ; weighted pondère par effectif. Un F1 binaire concerne la classe positive.

```python
from sklearn.metrics import f1_score
print("F1 macro :", f1_score(y_test, y_pred, average="macro", zero_division=0))
```

# Probabilités et règle de décision

Modifier le seuil change les classes prédites ; cela ne change pas les probabilités produites. Le seuil ci-dessous est fixé pour illustrer le cours, pas choisi en maximisant le score du test.

```python
probs = clf.predict_proba(X_test_modele)
indice_positif = np.flatnonzero(clf.classes_ == 1)[0]
proba_positive = probs[:, indice_positif]
y_pred_seuil = np.where(proba_positive > 0.4, 1, 0)
print(confusion_matrix(y_test, y_pred_seuil, labels=[0, 1]))
```

# ROC et AUC

Ces métriques utilisent les scores continus ; elles restent identiques lorsqu’on change seulement le seuil final.

```python
from sklearn.metrics import roc_curve, auc
fpr, tpr, seuils = roc_curve(y_test, proba_positive, pos_label=1)
aire_roc = auc(fpr, tpr)
plt.plot(fpr, tpr, label=f"AUC = {aire_roc:.2f}")
plt.plot([0, 1], [0, 1], linestyle="--")
plt.xlabel("Taux de faux positifs")
plt.ylabel("Taux de vrais positifs")
plt.legend()
plt.show()
```

## Paramètres utiles

- `y_test` : vérité terrain ; `y_pred` : classes prédites, dans le même ordre.
- `labels` : ordre des classes dans la matrice ; lignes = réel, colonnes = prédit.
- `average="macro"` : moyenne non pondérée des F1 par classe ; `"weighted"` pondère par effectif ; `"binary"` vise la classe positive en binaire.
- `zero_division=0` : retourne 0 quand une métrique est indéfinie ; examiner la cause au lieu de masquer le problème.
- `pos_label=1` : définit la classe positive ; `0.4` est le seuil illustratif du cours.
- `fpr`, `tpr` : taux de faux positifs et de vrais positifs ; `auc` intègre la courbe.

## Interprétation

Précision : parmi les positifs prédits, combien sont corrects ? Rappel : parmi les positifs réels, combien sont retrouvés ? F1 combine les deux. Une AUC élevée indique un bon classement relatif, pas une bonne calibration des probabilités.

# Pattern minimal

À partir des classes déjà prédites et de la vérité terrain :

```python
cm = confusion_matrix(y_test, y_pred)
rapport = classification_report(y_test, y_pred, zero_division=0)
print(cm)
print(rapport)
```



## Points de vigilance

- La partie ROC ci-dessus ne s’applique pas telle quelle au multiclasse.
- Les colonnes de predict_proba suivent classes_ ; ne pas supposer que la seconde correspond toujours au label 1.
- SVC exige probability=True avant fit pour utiliser predict_proba.
- Le test final ne sert ni à choisir le modèle, ni à choisir le seuil.


## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de classification.md`.

- [metrics.confusion_matrix](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.confusion_matrix.html)
- [metrics.classification_report](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.classification_report.html)
- [metrics.f1_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)
- [metrics.roc_curve](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_curve.html)
- [metrics.auc](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.auc.html)

