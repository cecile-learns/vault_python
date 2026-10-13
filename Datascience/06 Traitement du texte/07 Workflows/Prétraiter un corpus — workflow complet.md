# Prétraiter un corpus — workflow complet

## Pour faire quoi ?

Nettoyer, tokeniser, filtrer et raciniser un corpus avant sa vectorisation.

## Script réutilisable

```python
import re
from nltk.corpus import stopwords
from nltk.stem.snowball import PorterStemmer
from nltk.tokenize import word_tokenize

mots_vides = set(stopwords.words("english"))
stemmer = PorterStemmer()

def preparer_texte(texte):
    texte = texte.lower()
    texte = re.sub(r"[^a-z']+", " ", texte)
    tokens = word_tokenize(texte, language="english")
    tokens = [token for token in tokens if token not in mots_vides]
    tokens = [stemmer.stem(token) for token in tokens]
    return " ".join(tokens)

corpus_prepare = corpus.astype(str).apply(preparer_texte)
```

## À adapter

- langue ;
- motif de nettoyage ;
- stopwords du domaine ;
- stemming ou lemmatisation.

## Vigilances & tips

Tester le résultat sur plusieurs textes avant de traiter tout le corpus. Conserver le corpus brut.
