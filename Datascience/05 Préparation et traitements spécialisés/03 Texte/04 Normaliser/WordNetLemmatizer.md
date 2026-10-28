# `WordNetLemmatizer`

La lemmatisation (_lemmatisation_ en anglais) est une technique similaire à la racinisation mais plus avancée. Elle consiste à transformer des mots en leur lemme (_lemma_ en anglais), c'est-à-dire en un terme issu de l’usage ordinaire des locuteurs de la langue (forme dans le dictionnaire). Contrairement à la racinisation, la lemmatisation tient compte du sens sémantique des mots et peut produire des formes qui sont des mots valides.
## Syntaxe

```python
from nltk.stem import WordNetLemmatizer

# Initialiser un lemmatiseur wordnet_lemmatizer
wordnet_lemmatizer = WordNetLemmatizer()

#Calcul le lemme 
wordnet_lemmatizer.lemmatize('meeting', pos='v'), wordnet_lemmatizer.lemmatize('meeting', pos='n')

#('meet', 'meeting')
```




## Paramètres

`pos` précise notamment `"v"` pour un verbe et `"n"` pour un nom.

## Vigilances & tips

Cette ressource WordNet concerne principalement l'anglais.
La lemmatisation est généralement plus complexe et prend plus de temps que la racinisation en raison de sa sophistication linguistique, mais elle fournit des résultats plus précis.