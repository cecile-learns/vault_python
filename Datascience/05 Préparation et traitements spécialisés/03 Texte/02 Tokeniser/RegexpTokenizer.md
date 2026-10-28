# `RegexpTokenizer`

Possibilité de découper un corpus en gardant que des _tokens_ particuliers par le biais des expressions régulières.
## Syntaxe

```python
from nltk.tokenize import RegexpTokenizer

tokeniseur = RegexpTokenizer(r"[A-Za-zÀ-ÿ']{5,}")
tokens = tokeniseur.tokenize(texte.lower())
```

## Fonction

Tokenise en ne conservant que les éléments décrits par l'expression régulière.

|Paramètre|Rôle|
|---|---|
|`pattern`|Regex définissant les tokens à extraire|
|`gaps=False`|Si `True`, la regex définit les séparateurs plutôt que les tokens|
|`discard_empty=True`|Supprime les tokens vides|
|`flags`|Définit des options de fonctionnement de la regex|
### Les transformations possibles avant la tokenisation

Sur ton texte, tu peux notamment appliquer :

| Méthode       | Action                                            |
| ------------- | ------------------------------------------------- |
| `.lower()`    | Met en minuscules                                 |
| `.upper()`    | Met en majuscules                                 |
| `.strip()`    | Supprime les espaces au début et à la fin         |
| `.replace()`  | Remplace une chaîne par une autre                 |
| `.casefold()` | Normalisation de casse plus poussée que `lower()` |
|               |                                                   |

Documentation : [`RegexpTokenizer`](https://www.nltk.org/api/nltk.tokenize.RegexpTokenizer.html)
