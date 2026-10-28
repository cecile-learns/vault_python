# `cv2.adaptiveThreshold`

## Syntaxe

```python
image_seuillee = cv2.adaptiveThreshold(
    image_gris,
    255,
    adaptiveMethod=cv2.ADAPTIVE_THRESH_GAUSSIAN_C,
    thresholdType=cv2.THRESH_BINARY,
    blockSize=11,
    C=2,
)
#dans le cours, est ajouté en amont : filtre = cv2.GaussianBlur(img_gray, ksize = (3,3), sigmaX = 0)
```
![[Pasted image 20261023153513.png]]![[Pasted image 20261023153518.png]]
## Paramètres

- image source en niveaux de gris ;
- valeur maximale ;
- `adaptiveMethod` : moyenne locale ou moyenne gaussienne pondérée ;
- `thresholdType` : seuillage binaire normal ou inversé ;
- `blockSize` : taille impaire du voisinage local ;
- `C` : constante soustraite au seuil local calculé.

## Fonction

Calcule un seuil différent selon le voisinage de chaque pixel.

## Vigilances & tips

Utile lorsque l'éclairage varie dans l'image. `blockSize` doit être impair et supérieur à 1.

[Documentation officielle](https://docs.opencv.org/4.x/d7/d4d/tutorial_py_thresholding.html)
