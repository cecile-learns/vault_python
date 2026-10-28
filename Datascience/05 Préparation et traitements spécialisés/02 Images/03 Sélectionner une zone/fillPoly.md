dtyp# `cv2.fillPoly`

## Syntaxe

```python
sommets = np.array([[(x1, y1), (x2, y2), (x3, y3)]], dtype=np.int32)
masque = cv2.fillPoly(masque, sommets, color=255)
```

## Paramètres

- `masque` : image dans laquelle dessiner ;
- `sommets` : tableau d'entiers contenant une ou plusieurs listes de sommets ;
- `color=255` : valeur utilisée pour remplir le polygone dans un masque en niveaux de gris.

## Fonction

Remplit un ou plusieurs polygones dans une image ou un masque.

[Documentation officielle](https://docs.opencv.org/4.x/d6/d6e/group__imgproc__draw.html)
