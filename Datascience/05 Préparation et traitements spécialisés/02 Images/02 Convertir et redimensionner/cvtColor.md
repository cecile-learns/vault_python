# `cv2.cvtColor`

## Syntaxe

```python
image_rgb = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2RGB)
image_gris = cv2.cvtColor(image_bgr, cv2.COLOR_BGR2GRAY)
```

## Paramètres

- premier argument : image source ;
- code de conversion : transformation colorimétrique à appliquer.

## Fonction

Convertit une image d'un espace colorimétrique vers un autre.

## Vigilances & tips

`BGR2RGB` sert notamment avant un affichage Matplotlib. `BGR2GRAY` réduit l'image à un canal.

[Documentation officielle](https://docs.opencv.org/4.x/d8/d01/group__imgproc__color__conversions.html)
