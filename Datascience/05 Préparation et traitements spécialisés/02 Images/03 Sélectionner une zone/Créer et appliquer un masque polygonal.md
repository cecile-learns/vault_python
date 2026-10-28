# Créer et appliquer un masque polygonal

## Pour faire quoi ?

Conserver une région polygonale d'une image et masquer le reste.

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
masque = cv2.fillPoly(masque, sommets, color=255)
image_masquee = cv2.bitwise_and(image_gris, masque)
```

## À adapter

- coordonnées des sommets ;
- image source ;
- valeur de remplissage selon le type du masque.

## Vigilances & tips

Les coordonnées utilisent l'ordre `(x, y)`, soit `(colonne, ligne)`.

## Commandes utilisées

`shape`, `np.array`, `np.zeros_like`, `cv2.fillPoly`, `cv2.bitwise_and`.
