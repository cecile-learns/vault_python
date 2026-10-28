# `cv2.flip`

## Syntaxe

```python
image_verticale = cv2.flip(image_source, 0)
image_horizontale = cv2.flip(image_source, 1)
image_deux_axes = cv2.flip(image_source, -1)
```

## Paramètres

- premier argument : image source ;
- `0` : retournement vertical ;
- valeur positive : retournement horizontal ;
- valeur négative : retournement selon les deux axes.

## Fonction

Retourne une copie de l'image retournée selon l'axe demandé.

[Documentation officielle](https://docs.opencv.org/4.x/d2/de8/group__core__array.html)
