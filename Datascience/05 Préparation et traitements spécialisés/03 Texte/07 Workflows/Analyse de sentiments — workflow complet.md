# Analyse de sentiments — workflow complet

## Pour faire quoi ?

Vectoriser des textes, entraîner un classifieur puis évaluer ses prédictions.

## Script réutilisable

```python
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics import classification_report, confusion_matrix
from sklearn.model_selection import train_test_split

X_train_txt, X_test_txt, y_train, y_test = train_test_split(
    textes,
    cible,
    test_size=0.20,
    random_state=30,
    stratify=cible,
)

vectoriseur = TfidfVectorizer()
X_train = vectoriseur.fit_transform(X_train_txt)
X_test = vectoriseur.transform(X_test_txt)

modele = GradientBoostingClassifier(random_state=0)
modele.fit(X_train, y_train)
y_pred = modele.predict(X_test)

print(confusion_matrix(y_test, y_pred))
print(classification_report(y_test, y_pred, zero_division=0))
```

## Vigilances & tips

Le vectoriseur apprend uniquement sur le train. Comparer les métriques par classe et vérifier l'équilibre de la cible.


### Avec la normalisation - Stemmer

```python
#Importer le package pour la tokenization
from nltk.tokenize import word_tokenize

df["Text"] = df["Text"].apply(word_tokenize)
df.head()

# Importer stopwords de la classe nltk.corpus
from nltk.corpus import stopwords

# Initialiser la variable des mots vides
stop_words = set(stopwords.words('english'))
print(stop_words)

new_stop_words = [",", ".", "``", "@", "*", "(", ")", "...", "!", "?", "-", "_", ">", "<", ":", "/", "=", "--", "©", "~", ";", "\\", "\\\\"]

#Ajouter à la liste des stopwords des éléments de syntaxe que nous avons repéré et qui ne servent pas à l'analyse du texte
stop_words.update(new_stop_words)

#Définir la fonction stop_words_filtering
def stop_words_filtering(mots) : 
    tokens = []
    for mot in mots:
        if mot not in stop_words:
            tokens.append(mot)
    return tokens

df["Text"] = df["Text"].apply(stop_words_filtering)
df['Text'].head()

#Reconstitution des phrases après la tokenization
for i in range (0, len(df['Text'])):
    df.loc[i, 'Text'] = ' '.join(df.loc[i, 'Text'])

df['Text'].head()

#Importer le package re
import re

#Regex pour tous les '.', '..', '...', ...
for i in range (0, len(df['Text'])):
    df.loc[i, 'Text'] = re.sub(r"\.+", '', df.loc[i, 'Text'])

#Regex pour tous les chiffres et nombres
for i in range (0, len(df['Text'])):
    df.loc[i, 'Text'] = re.sub(r"[0-9]+", '', df.loc[i, 'Text'])

#Vérification que tous les éléments qu'on a voulu enlever ont bien été enlever du dataframe
filtered = [i for i in df['Text']]
print(filtered)

# Importer le package 
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

# Créer un vectorisateur 
vec_stem = TfidfVectorizer()

# Mettre à jour la valeur de X_train_stem et X_test_stem
X_train_stem = vec_stem.fit_transform(X_train_stem)
X_test_stem = vec_stem.transform(X_test_stem)

# Créer un classificateur clf_stem et entraîner le modèle sur l'ensemble d'entraînement
clf_stem = GradientBoostingClassifier(n_estimators=100, learning_rate=1.0, max_depth=1, random_state=0).fit(X_train_stem, y_train_stem)

# Calculer les prédictions 
y_pred_stem = clf_stem.predict(X_test_stem)

# Calcul et affichage de classification_report
print(classification_report(y_test_stem, y_pred_stem) )

# Calcul et affichage de la matrice de confusion
conf_matrix_stem = pd.crosstab(y_test_stem, y_pred_stem, rownames=['Classe réelle'], colnames=['Classe prédite'])
conf_matrix_stem

```


###  Avec la lemmatisation

```Python
#Importer le package pour la tokenization
from nltk.tokenize import word_tokenize

df["Text"] = df["Text"].apply(word_tokenize)
df.head()

# Importer stopwords de la classe nltk.corpus
from nltk.corpus import stopwords

# Initialiser la variable des mots vides
stop_words = set(stopwords.words('english'))
print(stop_words)

new_stop_words = [",", ".", "``", "@", "*", "(", ")", "...", "!", "?", "-", "_", ">", "<", ":", "/", "=", "--", "©", "~", ";", "\\", "\\\\"]

#Ajouter à la liste des stopwords des éléments de syntaxe que nous avons repéré et qui ne servent pas à l'analyse du texte
stop_words.update(new_stop_words)

#Définir la fonction stop_words_filtering
def stop_words_filtering(mots) : 
    tokens = []
    for mot in mots:
        if mot not in stop_words:
            tokens.append(mot)
    return tokens

df["Text"] = df["Text"].apply(stop_words_filtering)
df['Text'].head()

#Reconstitution des phrases après la tokenization
for i in range (0, len(df['Text'])):
    df.loc[i, 'Text'] = ' '.join(df.loc[i, 'Text'])

df['Text'].head()

#Importer le package re
import re

#Regex pour tous les '.', '..', '...', ...
for i in range (0, len(df['Text'])):
    df.loc[i, 'Text'] = re.sub(r"\.+", '', df.loc[i, 'Text'])

#Regex pour tous les chiffres et nombres
for i in range (0, len(df['Text'])):
    df.loc[i, 'Text'] = re.sub(r"[0-9]+", '', df.loc[i, 'Text'])

#Vérification que tous les éléments qu'on a voulu enlever ont bien été enlever du dataframe
filtered = [i for i in df['Text']]
print(filtered)

# Importer le package nécessaire
from nltk.stem import WordNetLemmatizer

# Initialiser un lemmatiseur
wordnet_lemmatizer = WordNetLemmatizer()

def lemmatisation(mots) :
    sortie = []
    for string in mots :
        radical = wordnet_lemmatizer.lemmatize(string)
        if (radical not in sortie) : sortie.append(radical)
    return sortie

# Séparer la variable explicative de la variable à prédire
X_lem, y_lem = df.Text, df.Sentiment

#Pour effectuer le lemmatising on sépare les phrase pour on les rassemble
X_lem = X_lem.str.split()

for i in range (0, len(X_lem)):
    X_lem[i] = lemmatisation(X_lem[i])
    X_lem[i] = ' '.join(X_lem[i])
    

# Séparer le jeu de données en données d'entraînement et données test 
X_train_lem, X_test_lem, y_train_lem, y_test_lem = train_test_split(X_lem, y_lem, test_size=0.2, random_state = 30)

vec_lem = TfidfVectorizer()

# Mettre à jour la valeur de X_train_lem et X_test_lem
X_train_lem = vec_lem.fit_transform(X_train_lem)
X_test_lem = vec_lem.transform(X_test_lem)

# Créer un classificateur clf_lem et entraîner le modèle sur l'ensemble d'entraînement
clf_lem = GradientBoostingClassifier(n_estimators=100, learning_rate=1.0, max_depth=1, random_state=0).fit(X_train_lem, y_train_lem)

# Calculer les prédictions 
y_pred_lem = clf_lem.predict(X_test_lem)

# Calcul et affichage de classification_report
print( classification_report(y_test_lem, y_pred_lem) )

# Calcul et affichage de la matrice de confusion
conf_matrix_lem = pd.crosstab(y_test_lem, y_pred_lem, rownames=['Classe réelle'], colnames=['Classe prédite'])
conf_matrix_lem

```