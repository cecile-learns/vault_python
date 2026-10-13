# Créer un nuage de mots — `WordCloud`

## Pattern

```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt

nuage = WordCloud(
    width=800,
    height=400,
    background_color="white",
    max_words=100,
    stopwords=mots_vides,
    max_font_size=50,
    random_state=42,
).generate(texte)

plt.figure(figsize=(10, 5))
plt.imshow(nuage, interpolation="bilinear")
plt.axis("off")
plt.show()
```

## Fonction

La taille des mots représente leur fréquence selon le traitement de WordCloud. Ce graphique est exploratoire et dépend fortement du nettoyage préalable.

Documentation : [`WordCloud`](https://amueller.github.io/word_cloud/generated/wordcloud.WordCloud.html)
