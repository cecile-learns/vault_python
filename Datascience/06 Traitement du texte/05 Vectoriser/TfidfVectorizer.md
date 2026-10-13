# `TfidfVectorizer`

## Pattern train / test

```python
from sklearn.feature_extraction.text import TfidfVectorizer

vectoriseur = TfidfVectorizer()
X_train_tfidf = vectoriseur.fit_transform(X_train_texte)
X_test_tfidf = vectoriseur.transform(X_test_texte)
```

## Paramètres utiles

- `stop_words` : mots vides à retirer ;
- `ngram_range` : unigrammes, bigrammes, etc. ;
- `min_df`, `max_df` : filtrage selon la fréquence documentaire ;
- `sublinear_tf=True` : remplace la fréquence brute par une échelle logarithmique.

## Vigilances & tips

Le TF-IDF pondère la fréquence ; il ne représente ni l'ordre complet ni le sens contextuel des mots.

Documentation : [`TfidfVectorizer`](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html)
