# `cv2.addWeighted`

## Syntaxe

```python
image_superposee = cv2.addWeighted(
    image_source,
    0.8,
    image_lignes,
    1.0,
    0.0,
)
```

## Paramètres

- première image et son poids `0.8` ;
- seconde image et son poids `1.0` ;
- dernier scalaire : valeur ajoutée à la combinaison, ici `0.0`.

## Fonction

Retourne une combinaison pondérée de deux images de mêmes dimensions et type.

[Documentation officielle](https://docs.opencv.org/4.x/d2/de8/group__core__array.html)
