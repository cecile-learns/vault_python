# `CountVectorizer`

## Pattern train / test

```python
from sklearn.feature_extraction.text import CountVectorizer

vectoriseur = CountVectorizer()
X_train_vect = vectoriseur.fit_transform(X_train_texte)
X_test_vect = vectoriseur.transform(X_test_texte)

noms_tokens = vectoriseur.get_feature_names_out()
```

## Paramètres utiles

- `stop_words` : mots vides à retirer ;
- `ngram_range` : tailles des groupes de mots ;
- `min_df`, `max_df` : fréquences documentaires minimale et maximale ;
- `max_features` : limite du vocabulaire.

## Vigilances & tips

Apprendre le vocabulaire uniquement avec le train. Le résultat est une matrice creuse.

Documentation : [`CountVectorizer`](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html)
