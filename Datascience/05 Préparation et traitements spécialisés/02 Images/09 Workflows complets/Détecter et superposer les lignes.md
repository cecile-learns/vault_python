# Détecter et superposer les lignes

## Pour faire quoi ?

Lisser une image, détecter ses contours, identifier des segments puis les superposer à l'image originale.

## Script réutilisable

```python
import cv2
import numpy as np

image_filtree = cv2.GaussianBlur(
    image_rgb,
    ksize=(3, 3),
    sigmaX=0,
)

contours = cv2.Canny(image_filtree, 100, 200)

lignes = cv2.HoughLinesP(
    contours,
    rho=3,
    theta=np.pi / 20,
    threshold=100,
    minLineLength=0,
    maxLineGap=20,
)

image_lignes = np.zeros_like(image_rgb)

if lignes is not None:
    for ligne in lignes:
        x1, y1, x2, y2 = ligne[0]
        cv2.line(
            image_lignes,
            (x1, y1),
            (x2, y2),
            color=(255, 0, 0),
            thickness=3,
        )

image_resultat = cv2.addWeighted(
    image_rgb,
    0.8,
    image_lignes,
    1.0,
    0.0,
)
```
![[Pasted image 20261023155735.png]]
## À adapter

- seuils de Canny ;
- paramètres de Hough ;
- couleur, épaisseur et poids de superposition.

## Vigilances & tips

Toujours vérifier que `lignes` n'est pas `None` avant de parcourir le résultat.

## Commandes utilisées

`cv2.GaussianBlur`, `cv2.Canny`, `cv2.HoughLinesP`, `np.zeros_like`, `cv2.line`, `cv2.addWeighted`.
