# `TfidfVectorizer`

## Pattern train / test

```python
from sklearn.feature_extraction.text import TfidfVectorizer

#Initialiser un vectorisateur vectorizer
vectorizer = TfidfVectorizer()

#Appliquer l'algorithme de numérotation
vectorizer.fit_transform(string)

#Récupérer les _tokens_ numérotés</code>
tokenized_tfidf=vectorizer.vocabulary_

#Pour afficher la représentation vectorielle d'une nouvelle phrase
vectorizer.transform(["une nouvelle phrase"]).toarray()


#Exemple du cours 
token=['souffrez', 'amour', 'cette', 'nuit', 'vous', 'réveille', 'soupirs', 'laissez', 'vous', 'enflammer', 'vous', 'dormez', 'trop', 'adorable', 'merveille', 'dormir', 'point', 'aimer']
vectorizer_tfidf = TfidfVectorizer()
vectorizer_tfidf.fit_transform(tokens)
tokenized_tfidf=vectorizer_tfidf.vocabulary_

print(tokenized_tfidf)
#{'souffrez': 12, 'amour': 2, 'cette': 3, 'nuit': 9, 'vous': 15, 'réveille': 11, 'soupirs': 13, 'laissez': 7, 'enflammer': 6, 'dormez': 4, 'trop': 14, 'adorable': 0, 'merveille': 8, 'dormir': 5, 'point': 10, 'aimer': 1}

print(vectorizer_tfidf.transform(["Dormez vous vous cette nuit ?"]).toarray())
#[[0.         0.         0.         0.         0.         0.
# 0.61791199 0.61791199 0.         0.         0.         0.
#0.         0.         0.         0.48617852]

#[0.         0.         0.         0.52565601 0.52565601 0.
#  1.         0.         0.         0.52565601 0.         0.
# 1.         0.         0.         0.41359071]]

```
![[Pasted image 20261014102731.png]]
## Paramètres utiles

- `stop_words` : mots vides à retirer ;
- `ngram_range` : unigrammes, bigrammes, etc. ;
- `min_df`, `max_df` : filtrage selon la fréquence documentaire ;
- `sublinear_tf=True` : remplace la fréquence brute par une échelle logarithmique.

## Vigilances & tips

Le TF-IDF pondère la fréquence ; il ne représente ni l'ordre complet ni le sens contextuel des mots.
- **TF** (Term Frequency), le score représentant la fréquence d'apparition d'un mot dans une phrase.
- **IDF** (Inverse Document Frequency), le score représentant la spécificité d'un mot dans un corpus de textes.
**Limitations du Bag of Words :** L'ordre des mots n'est pas pris en compte dans la représentation Bag of Words, ainsi cette représentation ne peut être utilisée pour des approches de type syntaxiques.
Documentation : [`TfidfVectorizer`](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html)

![[Pasted image 20261014102144.png]]