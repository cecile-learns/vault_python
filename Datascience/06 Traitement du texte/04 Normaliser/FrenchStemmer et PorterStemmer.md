# Raciniser — `FrenchStemmer` et `PorterStemmer`

## Français

```python
from nltk.stem.snowball import FrenchStemmer

stemmer = FrenchStemmer()
racines = [stemmer.stem(token) for token in tokens]
```

## Anglais

```python
from nltk.stem.snowball import PorterStemmer

stemmer = PorterStemmer()
racines = [stemmer.stem(token) for token in tokens]
```

## Fonction

Réduit les mots à une racine algorithmique, qui n'est pas nécessairement un mot valide.

Documentation : [stem NLTK](https://www.nltk.org/api/nltk.stem.html)
