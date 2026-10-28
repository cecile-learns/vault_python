
# Lire les courbes d’évaluation en Machine Learning

|Courbe|Ce qui varie sur X|Ce qu’on cherche|
|---|---|---|
|ROC|seuil de classification|discrimination|
|Cumulative Gain|% de population ciblée|efficacité du ciblage|
|Learning Curve|quantité de données train|biais / variance / généralisation|

## ROC Curve
``` Markdown
### Question posée
Le modèle discrimine-t-il bien les classes selon différents seuils ?

### Axes
X → taux de faux positifs
Y → taux de vrais positifs / rappel

### Bonne courbe
Proche du coin supérieur gauche.

### Référence
Diagonale = modèle aléatoire.

### À retenir
fait varier le seuil de décision
→ observe compromis vrais positifs / faux positifs

### Indicateur associé
AUC
```

```Python
from sklearn.metrics import roc_curve, auc

fpr, tpr, seuils = roc_curve(y_test, probs[:,1], pos_label=1)
roc_auc = auc(fpr, tpr);
```
![[Pasted image 20261002150637.png]]


## Cumulative Gains Curve
``` Markdown 
### Question posée
Si je cible les individus les mieux scorés, combien de positifs réels vais-je récupérer ?

### Axes
X → % de la population ciblée
Y → % des positifs réels récupérés

### Bonne courbe
Monte très vite vers 1.

### Référence
Diagonale = sélection aléatoire.

### À retenir
classe les individus du score le plus élevé au plus faible
→ mesure l’efficacité du ciblage
```

```Python
import matplotlib.pyplot as plt
import scikitplot as skplt

skplt.metrics.plot_cumulative_gain(y_test, y_probas);
```
![[Pasted image 20261002150552.png]]
![[Pasted image 20261002152006.png|328]]

## Learning Curve
``` Markdown 
### Question posée
Comment le modèle se comporte-t-il quand on augmente la quantité de données d’entraînement ?

### Axes
X → taille du jeu d’entraînement
Y → score / erreur

### Courbes
score train
score validation

### Lire les écarts

Train bon + validation mauvaise
→ variance élevée / sur-apprentissage

Train mauvais + validation mauvaise
→ biais élevé / sous-apprentissage

Train et validation convergent vers un bon niveau
→ généralisation correcte

### À retenir
taille des données
→ train vs validation
→ diagnostic biais / variance
```

![[Pasted image 20261002150711.png]]
```Python
from sklearn.model_selection import learning_curve
import matplotlib.pyplot as plt
%matplotlib inline 

train_sizes, train_scores, valid_scores = learning_curve(svm.SVC(kernel='linear', C= 1), data, target, train_sizes=[50, 80, 110, 140], cv=5)

plt.xlabel("Training examples")
plt.ylabel("Score")

train_sizes=[50, 70, 80, 100, 110, 118]

train_sizes, train_scores, test_scores = learning_curve(
    grid_clf, data, target, n_jobs=4, train_sizes=train_sizes)

train_scores_mean = np.mean(train_scores, axis=1)
train_scores_std = np.std(train_scores, axis=1)
test_scores_mean = np.mean(test_scores, axis=1)
test_scores_std = np.std(test_scores, axis=1)
plt.grid()

plt.fill_between(train_sizes, train_scores_mean - train_scores_std,
                 train_scores_mean + train_scores_std, alpha=0.1,
                 color="r")
plt.fill_between(train_sizes, test_scores_mean - test_scores_std,
                 test_scores_mean + test_scores_std, alpha=0.1, color="g")
plt.plot(train_sizes, train_scores_mean, 'o-', color="r",
         label="Training score")
plt.plot(train_sizes, test_scores_mean, 'o-', color="g",
         label="Cross-validation score")

plt.legend(loc="best")

```
