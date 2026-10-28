
```python
import pandas as pd
df=pd.read_csv('movies_comments.csv')
print(df.shape)
df.head(5)
```

![[Pasted image 20261014110311.png]]

```python
# Définir la variable text
text = ""
for comment in df.Text : 
    text += comment

# Importer stopwords de la classe nltk.corpus
from nltk.corpus import stopwords

# Initialiser la variable des mots vides
stop_words = set(stopwords.words('english'))
print(stop_words)
```
{'ve', "we'll", 'i', 'her', 'our', 'hasn', 'for', 'above', 'under', 'of', 'herself', "shouldn't", 'do', 'isn', "he'll", 'she', "she's", 'wasn', 'yours', 'what', 'those', "weren't", 'ourselves', "wouldn't", "he'd", "we'd", 'not', 'yourselves', 'won', "won't", 'very', 'both', 'doesn', 'any', 'nor', 'shan', 't', "wasn't", 'will', 'which', 'this', 'an', 'during', "hasn't", 'if', 'by', "i'll", 'having', 'couldn', 'out', "you've", 'from', 'ma', "she'd", 'that', 'are', 'mustn', 'don', 'each', 'me', 'a', 'haven', 'further', 'once', 'then', 'or', 'into', 'so', "hadn't", "couldn't", 'shouldn', 'between', 'himself', 'were', 're', 'where', 'am', 'his', "they'd", 'but', 'be', "should've", 'as', 'some', 'more', "i've", 'until', "they'll", "shan't", 'its', 'at', 'in', 'below', "aren't", 'few', 'such', 'ours', 'them', "mustn't", 'hadn', 'd', 'myself', 'again', "it's", 'while', 'now', 'over', 's', 'all', "they're", "doesn't", 'hers', "i'd", 'before', "didn't", 'to', 'aren', "i'm", 'did', 'itself', 'has', 'when', 'against', 'here', 'just', 'the', 'after', "we've", "you'd", 'these', 'through', 'why', 'wouldn', "you're", "it'd", 'only', "don't", "we're", 'y', 'being', 'him', "she'll", "they've", 'll', 'too', 'there', "mightn't", 'you', "isn't", 'themselves', 'have', 'we', 'on', 'with', "haven't", 'doing', 'o', 'and', 'other', 'than', 'been', 'it', "that'll", 'ain', 'no', 'can', 'he', 'their', 'your', 'had', 'm', 'didn', 'same', "you'll", 'does', 'is', 'about', 'mightn', "needn't", 'most', 'because', 'was', 'needn', "he's", 'how', 'off', 'theirs', 'who', "it'll", 'own', 'up', 'yourself', 'down', 'they', 'should', 'weren', 'whom', 'my'}

```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt
%matplotlib inline

wc = WordCloud(background_color="black", 
               max_words=100, 
               stopwords=stop_words, 
               max_font_size=50, 
               random_state=42)
               
# Générer et afficher le nuage de mots

plt.figure(figsize= (8,6)) # Initialisation d'une figure
wc.generate(text)           # "Calcul" du wordcloud
plt.imshow(wc) # Affichage
plt.show()
```
![[Pasted image 20261014110756.png]]

```python
# Mettre à jour la valeur de stop_words
mots_vides = ["mission", "impossible", "harry", "potter", "Da", "Vinci", "Mountain", "Brokeback", "Code"]
stop_words.update(mots_vides)

# Séparer df en données positives et négatives
df_pos = df[df.Sentiment == 1]
df_neg = df[df.Sentiment == 0]

# Afficher les quatre premières lignes de df_pos
df_pos.head(4)

# Définir les données positives et négatives de types string
text_pos = ""
for e in df_pos.Text : text_pos += e
text_neg = ""
for e in df_neg.Text : text_neg += e

# Tracer le nuage de mots
plot_word_cloud(text_pos, "coeur.png", "white")
plot_word_cloud(text_neg, "mal.jpg")
```
![[Pasted image 20261014111543.png]]![[Pasted image 20261014111548.png]]