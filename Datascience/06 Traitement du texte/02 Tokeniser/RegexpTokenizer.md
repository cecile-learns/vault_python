# `RegexpTokenizer`

## Syntaxe

```python
from nltk.tokenize import RegexpTokenizer

tokeniseur = RegexpTokenizer(r"[A-Za-zÀ-ÿ']{5,}")
tokens = tokeniseur.tokenize(texte.lower())
```

## Fonction

Tokenise en ne conservant que les éléments décrits par l'expression régulière.

Documentation : [`RegexpTokenizer`](https://www.nltk.org/api/nltk.tokenize.RegexpTokenizer.html)
