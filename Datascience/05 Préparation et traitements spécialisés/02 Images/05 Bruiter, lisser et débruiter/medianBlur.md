# `cv2.medianBlur`

## Syntaxe

```python
image_filtree = cv2.medianBlur(image_bruitee, ksize=3)
```

## Paramètres

- image source ;
- `ksize` : taille du voisinage carré, obligatoirement impaire et supérieure à 1.

## Fonction

Remplace chaque pixel par la médiane de son voisinage. Ce filtre est adapté au bruit impulsionnel de type sel et poivre.

[Documentation officielle](https://docs.opencv.org/4.x/d4/d86/group__imgproc__filter.html)
