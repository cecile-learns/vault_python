# Effectuer une rotation

## Pour faire quoi ?

Faire tourner une image autour de son centre en conservant les dimensions du cadre.

## Script réutilisable

```python
import cv2

def tourner_image(image_source, angle):
    hauteur, largeur = image_source.shape[:2]
    centre = (largeur / 2, hauteur / 2)

    matrice_rotation = cv2.getRotationMatrix2D(
        center=centre,
        angle=angle,
        scale=1,
    )

    return cv2.warpAffine(
        image_source,
        matrice_rotation,
        dsize=(largeur, hauteur),
    )

image_tournee = tourner_image(image_source, angle=90)
```

## À adapter

- angle ;
- échelle ;
- dimensions de sortie si les coins doivent rester visibles.

## Vigilances & tips

Le cadre conserve ici sa taille originale : certaines parties peuvent être coupées.

## Commandes utilisées

`shape`, `cv2.getRotationMatrix2D`, `cv2.warpAffine`.
