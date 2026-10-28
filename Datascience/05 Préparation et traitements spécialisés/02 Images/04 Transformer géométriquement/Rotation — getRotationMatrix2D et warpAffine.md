# Rotation — `getRotationMatrix2D` et `warpAffine`

## Pour faire quoi ?

Calculer puis appliquer une rotation autour du centre d'une image.

## Script réutilisable

```python
hauteur, largeur = image_source.shape[:2]
centre = (largeur / 2, hauteur / 2)

matrice_rotation = cv2.getRotationMatrix2D(
    center=centre,
    angle=90,
    scale=1,
)

image_tournee = cv2.warpAffine(
    image_source,
    matrice_rotation,
    dsize=(largeur, hauteur),
)
```

## À adapter

- `angle` : angle en degrés ;
- `scale` : facteur d'échelle ;
- centre et dimensions de sortie si la rotation ne doit pas conserver le cadre original.

## Vigilances & tips

`shape` renvoie `(hauteur, largeur)`, alors que le centre et `dsize` utilisent `(x, y)` et `(largeur, hauteur)`. Une rotation dans le cadre d'origine peut couper les coins.

## Commandes utilisées

`shape`, `cv2.getRotationMatrix2D`, `cv2.warpAffine`.

[Documentation officielle](https://docs.opencv.org/4.x/da/d54/group__imgproc__transform.html)
