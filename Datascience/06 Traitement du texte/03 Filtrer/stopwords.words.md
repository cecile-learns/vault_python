# `stopwords.words`

## Syntaxe

```python
from nltk.corpus import stopwords

mots_vides = set(stopwords.words("french"))
mots_vides.update([".", ","])

tokens_filtres = [token for token in tokens if token not in mots_vides]
```

## Vigilances & tips

Mettre le texte et les mots vides dans la même casse. Adapter la liste au domaine : un mot fréquent peut rester informatif dans un corpus spécialisé.
