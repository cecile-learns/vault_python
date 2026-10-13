# Analyse de sentiments — workflow complet

## Pour faire quoi ?

Vectoriser des textes, entraîner un classifieur puis évaluer ses prédictions.

## Script réutilisable

```python
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics import classification_report, confusion_matrix
from sklearn.model_selection import train_test_split

X_train_txt, X_test_txt, y_train, y_test = train_test_split(
    textes,
    cible,
    test_size=0.20,
    random_state=30,
    stratify=cible,
)

vectoriseur = TfidfVectorizer()
X_train = vectoriseur.fit_transform(X_train_txt)
X_test = vectoriseur.transform(X_test_txt)

modele = GradientBoostingClassifier(random_state=0)
modele.fit(X_train, y_train)
y_pred = modele.predict(X_test)

print(confusion_matrix(y_test, y_pred))
print(classification_report(y_test, y_pred, zero_division=0))
```

## Vigilances & tips

Le vectoriseur apprend uniquement sur le train. Comparer les métriques par classe et vérifier l'équilibre de la cible.
