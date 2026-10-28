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
![[Pasted image 20261014110936.png]]
## Fonction
La bibliothèque **wordcloud** implémente un algorithme permettant d'afficher un nuage de mots d'un texte. Cet algorithme regroupe les étapes suivantes :

- Tokeniser le texte passé en paramètre
- Filtrer les mots vides
- Calculer la fréquence des mots
- Représenter visuellement les mots-clefs les plus fréquents sous forme de nuage de mots, en voici un exemple :

La taille des mots représente leur fréquence selon le traitement de WordCloud. Ce graphique est exploratoire et dépend fortement du nettoyage préalable.

## Paramètre

|Paramètre|Rôle|Exemple|
|---|---|---|
|`width`|Largeur de l'image en pixels|`800`|
|`height`|Hauteur de l'image en pixels|`400`|
|`background_color`|Couleur du fond|`"white"`|
|`max_words`|Nombre maximal de mots affichés|`100`|
|`max_font_size`|Taille maximale des mots|`150`|
|`min_font_size`|Taille minimale des mots|`10`|
|`colormap`|Palette de couleurs Matplotlib|`"viridis"`|
|`color_func`|Fonction personnalisée pour colorer les mots|Fonction Python|
|`stopwords`|Ensemble de mots à exclure|`{"le", "la", "de"}`|
|`collocations`|Autorise les associations fréquentes de mots|`False`|
|`relative_scaling`|Influence de la fréquence sur la taille des mots|`0.5`|
|`prefer_horizontal`|Proportion souhaitée de mots horizontaux|`0.9`|
|`mask`|Masque NumPy définissant la forme du nuage|`mask_array`|
|`contour_width`|Épaisseur du contour du masque|`2`|
|`contour_color`|Couleur du contour|`"black"`|
|`random_state`|Graine pour reproduire le résultat|`42`|
|`regexp`|Regex utilisée pour identifier les mots|`r"\w+"`|
|`min_word_length`|Longueur minimale des mots retenus|`3`|
|`normalize_plurals`|Regroupe certaines formes plurielles anglaises|`True`|
### Trois points importants en Text Mining

- `stopwords` : indispensable à examiner pour le français, car les mots vides anglais sont utilisés par défaut. Il faut fournir une liste française adaptée.
    
- `collocations=False` : évite que WordCloud constitue automatiquement certaines expressions de deux mots. À adapter selon ce que tu souhaites analyser.
    
- `generate()` vs `generate_from_frequencies()` : le premier traite un texte ; le second utilise un dictionnaire de fréquences déjà calculées.


Documentation : [`WordCloud`](https://amueller.github.io/word_cloud/generated/wordcloud.WordCloud.html)
