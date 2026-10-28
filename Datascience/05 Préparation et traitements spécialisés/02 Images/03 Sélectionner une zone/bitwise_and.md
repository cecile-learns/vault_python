# `cv2.bitwise_and`

## Syntaxe

```python
image_masquee = cv2.bitwise_and(image_gris, masque)
```

## Paramètres

- premier argument : première image ;
- second argument : seconde image de mêmes dimensions et type.

## Fonction

Applique un ET bit à bit entre les pixels des deux images. Avec un masque composé de `0` et `255`, les zones à `0` deviennent noires et les zones à `255` conservent l'image.

## Vigilances & tips

Les dimensions et les types des deux tableaux doivent être compatibles.

[Documentation officielle](https://docs.opencv.org/4.x/d2/de8/group__core__array.html)
