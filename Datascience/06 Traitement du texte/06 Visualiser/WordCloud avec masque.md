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
```

## À adapter

- `chemin_masque` ;
- les mots vides ;
- les couleurs et le nombre de mots.

## Vigilances & tips

Le masque doit être converti en array. Dans la documentation actuelle, les zones blanches sont exclues du dessin.
