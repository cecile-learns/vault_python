
# stopwords

Donne la liste des mots vides (tellement commun qu'il est inutile de les indexer ou de les utiliser dans une recherche)
## Syntaxe

```python
# Importer stopwords de la classe nltk.corpus
from nltk.corpus import stopwords

# Initialiser la variable des mots vides
stop_words = set(stopwords.words('french'))
print(stop_words)
```

{'aurez', 'leur', 'étantes', 'suis', 'aurait', 'ton', 'un', 's', 'fussiez', 'eût', 'sois', 'fus', 'aurais', 'j', 'étant', 'ou', 'fussions', 'ayante', 'ayez', 'eux', 'avons', 'ayants', 'eut', 'soyons', 'étaient', 'serons', 'avaient', 'que', 'fusses', 'seraient', 'soit', 'des', 'on', 'auriez', 'aviez', 'serez', 'toi', 'étais', 'avez', 'sera', 'ce', 'mes', 'pour', 'étants', 'aies', 'serais', 'aux', 'ait', 'il', 'étées', 'mais', 'fut', 'eue', 'fût', 'eussions', 'serions', 'nous', 'la', 'ces', 'êtes', 'de', 'tu', 'au', 'étante', 'moi', 'aurions', 'notre', 'été', 'fusse', 'sont', 'étiez', 'et', 'avais', 'furent', 'ai', 'auront', 'ma', 'serait', 'lui', 'me', 'ayons', 'étée', 'aura', 'ayant', 'y', 'par', 'était', 'les', 'soyez', 'c', 'l', 'en', 'même', 'serai', 'pas', 'est', 'seriez', 'ont', 'soient', 'eues', 'vos', 'avions', 'eurent', 'étions', 'eûtes', 'eussiez', 'seront', 'avait', 'auraient', 'dans', 'seras', 'son', 'ne', 'aurai', 'ayantes', 'sur', 'ses', 'eûmes', 'votre', 'se', 'aient', 'sa', 't', 'eussent', 'ta', 'tes', 'ils', 'je', 'fûmes', 'mon', 'auras', 'd', 'n', 'fussent', 'eusses', 'aurons', 'qu', 'te', 'avec', 'eus', 'qui', 'sommes', 'eusse', 'nos', 'fûtes', 'vous', 'une', 'm', 'du', 'le', 'elle', 'as', 'à', 'étés', 'eu', 'es', 'aie'}

```python
#Ajout d'un mot vide
stop_words.update([",", "."])

def stop_words_filtering(mots) : 
    tokens = []
    for mot in mots:
        if mot not in stop_words:
            tokens.append(mot)
    return tokens

print(stop_words_filtering(mots)) 
```
## Paramètres

- `language` : modèle linguistique utilisé.

## Vigilances & tips

