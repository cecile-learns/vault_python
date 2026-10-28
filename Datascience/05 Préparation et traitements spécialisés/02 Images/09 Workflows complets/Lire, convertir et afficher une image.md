# Lire, convertir et afficher une image

## Pour faire quoi ?

Charger une image avec OpenCV puis l'afficher avec Matplotlib dans les bonnes couleurs.

## Script réutilisable

```python
import cv2
import matplotlib.pyplot as plt

image_bgr = cv2.imread("chemin/image.jpg", cv2.IMREAD_COLOR)

if image_bgr is None:
    raise FileNotFoundError("Image introuvable ou illisible")

image_rgb = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2RGB)

print("Dimensions :", image_rgb.shape)

plt.imshow(image_rgb)
plt.xticks([])
plt.yticks([])
plt.show()
```

## À adapter

- chemin du fichier ;
- lecture couleur ou niveaux de gris.

## Vigilances & tips

Ne pas appliquer la conversion BGR vers RGB à une image déjà chargée en niveaux de gris.

## Commandes utilisées

`cv2.imread`, `cv2.cvtColor`, `shape`, `plt.imshow`.
