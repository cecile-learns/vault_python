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


### Avec la normalisation - Stemmer

```python
# ============================================================
# 1. IMPORTS
# ============================================================

import re
import pandas as pd

from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from nltk.stem import PorterStemmer

from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.metrics import classification_report, confusion_matrix


# ============================================================
# 2. INITIALISATION DU PRÉTRAITEMENT
# ============================================================

stop_words = set(stopwords.words("english"))

# Conserver les négations importantes pour l'analyse de sentiments
stop_words -= {"no", "nor", "not"}

stemmer = PorterStemmer()


# ============================================================
# 3. FONCTION DE NETTOYAGE ET STEMMING
# ============================================================

def preprocess_text_stem(text):
    # Passage en minuscules
    text = str(text).lower()

    # Suppression des nombres
    text = re.sub(r"\d+", "", text)

    # Suppression de la ponctuation
    text = re.sub(r"[^\w\s]", " ", text)

    # Tokenisation
    tokens = word_tokenize(text)

    # Suppression des stopwords et stemming
    tokens = [
        stemmer.stem(token)
        for token in tokens
        if token not in stop_words
    ]

    # Reconstruction du texte
    return " ".join(tokens)


# ============================================================
# 4. PRÉPARATION DES VARIABLES
# ============================================================

X_stem = df["Text"].fillna("").apply(preprocess_text_stem)
y_stem = df["Sentiment"]

print(X_stem.head())


# ============================================================
# 5. SÉPARATION TRAIN / TEST
# ============================================================

X_train_stem, X_test_stem, y_train_stem, y_test_stem = train_test_split(
    X_stem,
    y_stem,
    test_size=0.2,
    random_state=30,
    stratify=y_stem
)


# ============================================================
# 6. VECTORISATION TF-IDF
# ============================================================

vec_stem = TfidfVectorizer()

X_train_stem_tfidf = vec_stem.fit_transform(X_train_stem)
X_test_stem_tfidf = vec_stem.transform(X_test_stem)


# ============================================================
# 7. ENTRAÎNEMENT DU MODÈLE
# ============================================================

clf_stem = GradientBoostingClassifier(
    n_estimators=100,
    learning_rate=1.0,
    max_depth=1,
    random_state=0
)

clf_stem.fit(X_train_stem_tfidf, y_train_stem)


# ============================================================
# 8. PRÉDICTIONS ET ÉVALUATION
# ============================================================

y_pred_stem = clf_stem.predict(X_test_stem_tfidf)

print("Classification report :")
print(classification_report(y_test_stem, y_pred_stem))

print("\nMatrice de confusion :")

conf_matrix_stem = pd.crosstab(
    pd.Series(y_test_stem.to_numpy(), name="Classe réelle"),
    pd.Series(y_pred_stem, name="Classe prédite")
)

print(conf_matrix_stem)

```


###  Avec la lemmatisation

```Python
# ============================================================
# 1. IMPORTS
# ============================================================

import re
import pandas as pd

from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from nltk.stem import WordNetLemmatizer

from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.metrics import classification_report, confusion_matrix


# ============================================================
# 2. INITIALISATION DU PRÉTRAITEMENT
# ============================================================

stop_words = set(stopwords.words("english"))

# Ponctuation et caractères supplémentaires à exclure
new_stop_words = {
    ",", ".", "``", "@", "*", "(", ")", "...",
    "!", "?", "-", ">", "<", ":", "/", "=",
    "--", "©", "~", ";", "\\", "\\\\"
}

stop_words.update(new_stop_words)

lemmatizer = WordNetLemmatizer()


# ============================================================
# 3. FONCTION DE NETTOYAGE ET LEMMATISATION
# ============================================================

def preprocess_text(text):
    # Passage en minuscules
    text = str(text).lower()

    # Suppression des nombres
    text = re.sub(r"\d+", "", text)

    # Suppression de la ponctuation
    text = re.sub(r"[^\w\s]", " ", text)

    # Tokenisation
    tokens = word_tokenize(text)

    # Suppression des stopwords et lemmatisation
    tokens = [
        lemmatizer.lemmatize(token)
        for token in tokens
        if token not in stop_words
    ]

    # Reconstruction du texte
    return " ".join(tokens)


# ============================================================
# 4. PRÉPARATION DES VARIABLES
# ============================================================

X = df["Text"].fillna("").apply(preprocess_text)
y = df["Sentiment"]

# Vérification
print(X.head())


# ============================================================
# 5. SÉPARATION TRAIN / TEST
# ============================================================

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=30,
    stratify=y
)


# ============================================================
# 6. VECTORISATION TF-IDF
# ============================================================

vectorizer = TfidfVectorizer()

X_train_tfidf = vectorizer.fit_transform(X_train)
X_test_tfidf = vectorizer.transform(X_test)


# ============================================================
# 7. ENTRAÎNEMENT DU MODÈLE
# ============================================================

clf = GradientBoostingClassifier(
    n_estimators=100,
    learning_rate=1.0,
    max_depth=1,
    random_state=0
)

clf.fit(X_train_tfidf, y_train)


# ============================================================
# 8. PRÉDICTIONS ET ÉVALUATION
# ============================================================

y_pred = clf.predict(X_test_tfidf)

print("Classification report :")
print(classification_report(y_test, y_pred))

print("\nMatrice de confusion :")
conf_matrix = pd.crosstab(
    pd.Series(y_test.to_numpy(), name="Classe réelle"),
    pd.Series(y_pred, name="Classe prédite")
)

print(conf_matrix)
```