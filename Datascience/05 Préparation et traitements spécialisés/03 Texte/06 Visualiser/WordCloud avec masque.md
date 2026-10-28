# Créer un WordCloud avec un masque

## Pattern

```python
import numpy as np
from PIL import Image
from wordcloud import WordCloud

masque = np.array(Image.open(chemin_masque))

nuage = WordCloud(
    background_color="white",
    max_words=200,
    stopwords=mots_vides,
    mask=masque,
    max_font_size=70,
    random_state=42,
).generate(texte)

#Exemple du cours
def plot_word_cloud(text, masque, background_color = "black") :
    # Définir un masque
    mask_coloring = np.array(Image.open(str(masque)))

    # Définir le calque du nuage des mots
    wc = WordCloud(width=800, height=400, background_color=background_color, max_words=200, stopwords=stop_words, mask = mask_coloring, max_font_size=70, random_state=42)

    # Générer et afficher le nuage de mots
    plt.figure(figsize= (10,5))
    wc.generate(text)
    plt.imshow(wc)
    plt.show()

plot_word_cloud(text, "iron.jpg")

# Code d'affichage du masque
import matplotlib.image as mpimg
img = mpimg.imread("iron.jpg")
plt.imshow(img)
plt.show()
```
![[Pasted image 20261014110909.png]]![[Pasted image 20261014110914.png]]
## À adapter

- `chemin_masque` ;
- les mots vides ;
- les couleurs et le nombre de mots.

## Vigilances & tips

Le masque doit être converti en array. Dans la documentation actuelle, les zones blanches sont exclues du dessin.
