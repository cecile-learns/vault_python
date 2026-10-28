# Raciniser — `FrenchStemmer` et `PorterStemmer`

La racinisation ou la désuffixation (_stemming_ en anglais) est une technique de transformation des mots en leur radical ou racine (_stem_ en anglais). La racine d'un mot correspond à la partie du mot restante une fois que l’on a supprimé ses préfixes et suffixes. Ainsi, il est parfois pertinent de rassembler tous les mots d'une même famille autour de la racine pour des analyses de textes.
## Français

```python
from nltk.stem.snowball import FrenchStemmer

```python
# Initialiser un racinisateur porter_stemmer
stemmer = FrenchStemmer()

# Calculer la racine du mot **word**
racine = porter_stemmer.stem(word)

#Exemple du cours : 
#  Définir la fonction stemming
def stemming(mots) :
    sortie = []
    for string in mots :
        radical = stemmer.stem(string)
        if (radical not in sortie) : sortie.append(radical)
    return sortie

# Appliquer la fonction stemming à la variable tokens
print(stemming(tokens))
```




## Anglais

```python
f# Importer le package 
from nltk.stem.snowball import PorterStemmer

# Créer un racinisateur
stemmer = PorterStemmer()

#  Définir la fonction stemming
def stemming(mots) :
    sortie = []
    for string in mots :
        radical = stemmer.stem(string)
        if (radical not in sortie) : sortie.append(radical)
    return sortie

# Séparer la variable explicative de la variable à prédire
X_stem, y_stem = df.Text, df.Sentiment

#Pour effectuer le stemming, il faut décomposer les phrases en suite de mots
X_stem = X_stem.str.split()
for i in range (0, len(X_stem)):
    X_stem[i] = stemming(X_stem[i])
    X_stem[i] = ' '.join(X_stem[i]) #On rassemble ici notre suite de mots en phrases
    
# Séparer le jeu de données en données d'entraînement et données test 
X_train_stem, X_test_stem, y_train_stem, y_test_stem = train_test_split(X_stem, y_stem, test_size=0.2, random_state = 30)
```

## Fonction

Réduit les mots à une racine algorithmique, qui n'est pas nécessairement un mot valide.

Documentation : [stem NLTK](https://www.nltk.org/api/nltk.stem.html)
