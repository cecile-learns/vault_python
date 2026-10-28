# ROC et AUC

## Quand l’utiliser ?

Évaluation d'un modèle de **classification**, principalement binaire.

La courbe ROC permet d'étudier la capacité du modèle à **séparer les deux classes lorsque le seuil de classification varie**.

Elle peut être utilisée avec différents modèles capables de produire une probabilité ou un score continu : régression logistique, SVM, Random Forest, etc.

---

## Principe

Un classifieur peut produire un score ou une probabilité :

```
probabilité classe positive = 0.72
```

Avec un seuil de `0.5` :

```
0.72 > 0.5
→ classe positive
```

Mais avec un seuil de `0.8` :

```
0.72 < 0.8
→ classe négative
```

Changer le seuil modifie donc les VP, FP, VN et FN de la [[06 Modélisation ML/Évaluation et diagnostic/Matrice de confusion]].

La courbe ROC examine ce comportement pour différents seuils.

---

## Axes de la courbe ROC

### TPR — True Positive Rate

Correspond au **rappel / sensibilité** :

[  
TPR = \frac{VP}{VP+FN}  
]

→ proportion de positifs réels correctement détectés.

### FPR — False Positive Rate

[  
FPR = \frac{FP}{FP+VN}  
]

→ proportion de négatifs réels incorrectement déclarés positifs.

La ROC représente :

```
TPR
↑
│             ●
│          ●
│       ●
│    ●
│  ●
│╱
└────────────────→ FPR
```

On cherche globalement à se rapprocher du **coin supérieur gauche** :

```
TPR élevé
+
FPR faible
```

---

# Pattern

## Obtenir les scores

Pour un modèle disposant de `predict_proba` :

```python
y_score = modele.predict_proba(X_test)[:, 1]
```

Pour certains modèles utilisant `decision_function` :

```python
y_score = modele.decision_function(X_test)
```

## Tracer la ROC

```python
from sklearn.metrics import RocCurveDisplay

RocCurveDisplay.from_predictions(
    y_test,
    y_score
)
```

## Calculer l'AUC

```python
from sklearn.metrics import roc_auc_score

auc = roc_auc_score(y_test, y_score)
print("AUC :", auc)
```

---

## AUC — Area Under the Curve

L'AUC résume la courbe ROC par un nombre.

```
AUC ≈ 0.5
→ capacité de discrimination proche du hasard

AUC plus proche de 1
→ meilleure capacité à classer les positifs
  au-dessus des négatifs
```

Une autre lecture utile :

> L'AUC mesure la capacité du modèle à donner généralement un score plus élevé à un positif qu'à un négatif.

---

## Interprétation

### ROC

Elle répond principalement à :

> **Quel compromis entre détection des positifs et fausses alertes puis-je obtenir en faisant varier le seuil ?**

### AUC

Elle répond plutôt à :

> **Quelle est la capacité globale du modèle à discriminer les deux classes, indépendamment d'un seuil particulier ?**

---

## Que faire du résultat ?

```
ROC / AUC
    ↓
Le modèle discrimine-t-il suffisamment
les deux classes ?
    ↓
oui
    ↓
Quel seuil utiliser ?
    ↓
Quel est le coût métier
des FP et FN ?
    ↓
[[Matrice de confusion]]
+
[[Précision - Rappel - F1]]
```

La ROC ne choisit donc **pas automatiquement le seuil opérationnel**.

Le seuil doit être choisi en fonction du problème : accepter davantage de faux positifs peut être pertinent si manquer un positif est beaucoup plus grave, et inversement.

---

## Points de vigilance

- Utiliser les **scores/probabilités**, pas directement les classes produites par `predict`, pour construire la ROC.
    
- Une AUC élevée ne garantit pas qu'un seuil donné produit une bonne précision ou un bon rappel.
    
- ROC/AUC ne remplace pas la matrice de confusion.
    
- Avec des classes très déséquilibrées, compléter l'analyse par précision/rappel et éventuellement une courbe Precision-Recall.
    
- Comparer les modèles sur les mêmes données d'évaluation.
    
- Une bonne discrimination statistique ne signifie pas automatiquement que le modèle est utilisable métier.
    

