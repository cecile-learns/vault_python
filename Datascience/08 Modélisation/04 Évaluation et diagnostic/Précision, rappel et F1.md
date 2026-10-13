# Précision - Rappel - F1

## Quand l’utiliser ?

Évaluation d'un modèle de **classification**, notamment lorsque toutes les erreurs n'ont pas la même importance.

Ces métriques sont calculées à partir de la Matrice de confusion

---

## Principe

### Précision

> **Parmi ce que le modèle prédit positif, combien sont réellement positifs ?**

[  
Precision = \frac{VP}{VP+FP}  
]

```
          PRÉDIT POSITIF
               ↓
        ┌──────────────┐
        │ VP    +   FP │
        └──────────────┘
          ↑
     combien sont
   vraiment positifs ?
```

Une précision élevée signifie : **quand le modèle annonce positif, il se trompe rarement.**

---

### Rappel — Recall / Sensibilité

> **Parmi tous les positifs réels, combien le modèle en retrouve-t-il ?**

[  
Recall = \frac{VP}{VP+FN}  
]

```
          RÉEL POSITIF
               ↓
        ┌──────────────┐
        │ VP    +   FN │
        └──────────────┘
          ↑
     combien ont été
        retrouvés ?
```

Un rappel élevé signifie : **le modèle manque peu de positifs.**

---

### F1-score

Combine précision et rappel :

[  
F1 = 2 \times  
\frac{Precision \times Recall}  
{Precision + Recall}  
]

Le F1 est pénalisé lorsque l'une des deux métriques est faible.

---

# Pattern

```python
from sklearn.metrics import precision_score, recall_score, f1_score

precision = precision_score(y_test, y_pred)
recall = recall_score(y_test, y_pred)
f1 = f1_score(y_test, y_pred)

print("Précision :", precision)
print("Rappel :", recall)
print("F1 :", f1)
```

Pour obtenir plusieurs métriques :

```python
from sklearn.metrics import classification_report

print(classification_report(y_test, y_pred))
```

---

## Interprétation

### Faux positifs problématiques

```
Beaucoup de FP
     ↓
précision diminue
```

→ surveiller/privilégier la **précision**.

Question :

> Quand mon modèle annonce positif, puis-je lui faire confiance ?

---

### Faux négatifs problématiques

```
Beaucoup de FN
     ↓
rappel diminue
```

→ surveiller/privilégier le **rappel**.

Question :

> Mon modèle retrouve-t-il suffisamment des positifs existants ?

---

### Les deux sont importants

→ regarder précision **et** rappel.

Le **F1** fournit un résumé lorsque l'on souhaite un compromis entre les deux.

```
Quelle erreur est la plus coûteuse ?
          │
     ┌────┴────┐
     ↓         ↓
    FP         FN
     ↓         ↓
PRÉCISION    RAPPEL
     \         /
      \       /
       ↓     ↓
          F1
 si compromis pertinent
```

---

## Points de vigilance

- Un F1 élevé n'indique pas nécessairement que le modèle répond au besoin métier : il faut connaître le coût des FP et FN.
    
- Précision et rappel dépendent du **seuil de classification**.
    
- Augmenter le rappel peut diminuer la précision, et inversement.
    
- En classification multiclasse, préciser la méthode d'agrégation (`macro`, `micro`, `weighted`).
    
- Ne pas choisir automatiquement la métrique ayant la valeur la plus élevée : choisir celle qui correspond au problème.
    

