# `CountVectorizer`

## Pattern train / test

```python
from sklearn.feature_extraction.text import CountVectorizer

vectoriseur = CountVectorizer()

X_train_vect = vectoriseur.fit_transform(X_train)
X_test_vect = vectoriseur.transform(X_test)

noms_tokens = vectoriseur.get_feature_names_out()


#Exemple de cours
token=['souffrez', 'amour', 'cette', 'nuit', 'vous', 'réveille', 'soupirs', 'laissez', 'vous', 'enflammer', 'vous', 'dormez', 'trop', 'adorable', 'merveille', 'dormir', 'point', 'aimer']

vectorizer.fit_transform(tokens)
tokenized = vectorizer.vocabulary_
print(tokenized)

#{'souffrez': 12, 'amour': 2, 'cette': 3, 'nuit': 9, 'vous': 15, 'réveille': 11, 'soupirs': 13, 'laissez': 7, 'enflammer': 6, 'dormez': 4, 'trop': 14, 'adorable': 0, 'merveille': 8, 'dormir': 5, 'point': 10, 'aimer': 1}

vectorizer.transform(["laissez-vous enflammer", "dormez vous cette nuit ?"]).toarray()
#array([[0, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1],
  #     [0, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1]])
```

![[Pasted image 20261014101511.png]]


## Paramètres utiles

- `stop_words` : mots vides à retirer ;
- `ngram_range` : tailles des groupes de mots ;
- `min_df`, `max_df` : fréquences documentaires minimale et maximale ;
- `max_features` : limite du vocabulaire.

## Vigilances & tips

Apprendre le vocabulaire uniquement avec le train. Le résultat est une matrice creuse.
**Limitations du Bag of Words :** L'ordre des mots n'est pas pris en compte dans la représentation Bag of Words, ainsi cette représentation ne peut être utilisée pour des approches de type syntaxiques.

Documentation : [`CountVectorizer`](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html)


![[Pasted image 20261014102119.png]]