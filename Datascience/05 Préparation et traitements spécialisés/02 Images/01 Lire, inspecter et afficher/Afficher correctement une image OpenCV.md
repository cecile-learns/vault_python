# Afficher correctement une image OpenCV

## Pour faire quoi ?

Lire une image couleur avec OpenCV puis l'afficher avec Matplotlib sans inverser le rouge et le bleu.

## Script réutilisable

```python
import cv2
import matplotlib.pyplot as plt

image_bgr = cv2.imread("chemin/image.jpg", cv2.IMREAD_COLOR)

if image_bgr is None:
    raise FileNotFoundError("Image introuvable ou illisible")

image_rgb = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2RGB)

plt.imshow(image_rgb)
plt.xticks([])
plt.yticks([])
plt.show()
```

## À adapter

- chemin du fichier ;
- mode couleur ou niveaux de gris.

## Vigilances & tips

Pour une image en niveaux de gris, utiliser `plt.imshow(image_gris, cmap="gray")`.

## Commandes utilisées

`cv2.imread`, `cv2.cvtColor`, `plt.imshow`, `plt.xticks`, `plt.yticks`, `plt.show`.
