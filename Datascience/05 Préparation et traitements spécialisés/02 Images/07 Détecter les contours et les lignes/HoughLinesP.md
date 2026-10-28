# `cv2.HoughLinesP`

## Syntaxe

```python
lignes = cv2.HoughLinesP(
    contours,
    rho=3,
    theta=np.pi / 20,
    threshold=100,
    minLineLength=0,
    maxLineGap=20,
)
```

## Paramètres

Différents arguments sont utilisés dans la fonction :

- **dst**: Sortie du détecteur de formes. Il devrait s'agir d'une image en niveaux de gris (bien qu'il s'agisse en fait d'une image binaire)
- **lignes** : Un vecteur qui va stocker les paramètres (x_{début}, y_{début}, x_{fin}, y_{fin}) des lignes détectées
- **rho** : La résolution du paramètre r en pixels.
- **theta** : La résolution du paramètre \theta en radians.
- **threshold**: Le nombre minimum d'intersections pour "détecter" une ligne
- **minLinLength**: Le nombre minimum de points qui peuvent former une ligne. Les lignes ayant moins que ce nombre de points ne sont pas prises en compte.
- **maxLineGap**: L'écart maximum entre deux points à considérer dans une même ligne.

## Fonction

Retourne les extrémités des segments détectés par la transformée probabiliste de Hough.

## Vigilances & tips

Le résultat peut être `None` lorsqu'aucune ligne ne dépasse le seuil.

[Documentation officielle](https://docs.opencv.org/4.x/d9/db0/tutorial_hough_lines.html)
