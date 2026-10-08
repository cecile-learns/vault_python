
# Matrice de confusion

## Quand l’utiliser ?

Évaluation d'un modèle de **classification**.

Permet de comprendre **quelles erreurs fait le modèle**, et pas seulement combien de prédictions sont correctes.

> À utiliser après avoir obtenu les prédictions `y_pred`.

---

## Principe

Pour une classification binaire, chaque prédiction appartient à l'un des quatre cas :

- **VP** : positif correctement détecté.
    
- **VN** : négatif correctement détecté.
    
- **FP** : négatif prédit positif → **fausse alerte**.
    
- **FN** : positif prédit négatif → **positif manqué**.
    

> `Faux` signifie que la **prédiction est incorrecte**.

---

# Pattern

## Calculer la matrice

```python
from sklearn.metrics import confusion_matrix

y_pred = modele.predict(X_test)

cm = confusion_matrix(y_test, y_pred)
print(cm)
```

## La visualiser

```python
from sklearn.metrics import ConfusionMatrixDisplay

ConfusionMatrixDisplay.from_predictions(
    y_test,
    y_pred
)
```

---

## Lecture

La matrice permet surtout de distinguer deux types d'erreurs :

### Beaucoup de faux positifs

```
réel négatif
mais
prédit positif
```

Le modèle déclenche trop de **fausses alertes**.

→ regarder notamment la [[Précision - Rappel - F1#Précision|précision]].

### Beaucoup de faux négatifs

```
réel positif
mais
prédit négatif
```

Le modèle manque des cas positifs.

→ regarder notamment le [[Précision - Rappel - F1#Rappel|rappel]].

---

## Interprétation

La question n'est pas :

> Quel modèle fait le moins d'erreurs ?

mais également :

> **Quelles erreurs sont les plus problématiques dans mon problème métier ?**

Exemple dépistage :

```
FN
→ personne malade classée saine
→ cas manqué

FP
→ personne saine classée malade
→ fausse alerte
```

Les conséquences ne sont pas nécessairement équivalentes.

La matrice de confusion sert donc de base pour choisir les métriques adaptées et éventuellement ajuster le seuil de classification.

---

## Points de vigilance

- Toujours identifier clairement quelle classe est considérée comme **positive**.
    
- Une bonne accuracy peut masquer beaucoup d'erreurs sur une classe minoritaire.
    
- Comparer les nombres bruts peut être trompeur lorsque les classes sont très déséquilibrées.
    
- La matrice dépend du **seuil de classification** : modifier le seuil modifie VP, VN, FP et FN.
    
- En multiclasses, la matrice comporte une ligne et une colonne par classe.
    

