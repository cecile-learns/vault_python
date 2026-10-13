# `WordNetLemmatizer`

## Syntaxe

```python
from nltk.stem import WordNetLemmatizer

lemmatiseur = WordNetLemmatizer()
lemme_verbe = lemmatiseur.lemmatize("meeting", pos="v")
lemme_nom = lemmatiseur.lemmatize("meeting", pos="n")
```

## Paramètres

`pos` précise notamment `"v"` pour un verbe et `"n"` pour un nom.

## Vigilances & tips

Cette ressource WordNet concerne principalement l'anglais.
