# Isoler une région avec un masque

## Pour faire quoi ?

Définir une zone polygonale et masquer tout ce qui se trouve à l'extérieur.

## Script réutilisable

```python
import cv2
import numpy as np

hauteur, largeur = image_gris.shape

sommets = np.array(
    [[
        (0, hauteur),
        (largeur // 2, hauteur // 2),
        (largeur, hauteur),
    ]],
    dtype=np.int32,
)

masque = np.zeros_like(image_gris)
cv2.fillPoly(masque, sommets, color=255)

region_interet = cv2.bitwise_and(image_gris, masque)
```

## À adapter

- sommets du polygone ;
- image source ;
- forme et nombre de canaux du masque.

## Vigilances & tips

Vérifier les dimensions du masque et afficher celui-ci seul avant de l'appliquer.

## Commandes utilisées

`shape`, `np.array`, `np.zeros_like`, `cv2.fillPoly`, `cv2.bitwise_and`.
