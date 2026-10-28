# `cv2.threshold`

## Syntaxe

```python
seuil_utilise, image_seuillee = cv2.threshold(
    image_gris,
    115,
    255,
    cv2.THRESH_BINARY,
)

plt.figure(figsize = (8,5))
plt.imshow(image_seuillee, cmap='gray')
plt.xticks([])
plt.yticks([])

plt.show();
```
![[Pasted image 20261023153153.png]]![[Pasted image 20261023153157.png]]
## Paramètres

- image source en niveaux de gris ;
- `115` : seuil global ;
- `255` : valeur maximale attribuée par le seuillage ;
- type : `THRESH_BINARY`, `THRESH_BINARY_INV`, `THRESH_TRUNC`, `THRESH_TOZERO` ou `THRESH_TOZERO_INV`.

- **cv2.THRESH_BINARY** : Si l'intensité du pixel est supérieure au seuil fixé, la valeur est fixée à 255, sinon à 0 (noir).
- **cv2.THRESH_BINARY_INV** : Le même principe que cv2.THRESH_BINARY mais inversé.
- **cv2.THRESH_TRUNC** : Si la valeur de l'intensité du pixel est supérieure au seuil, elle est tronquée au seuil. Les valeurs des pixels sont fixées de manière à être identiques au seuil. Toutes les autres valeurs restent les mêmes.
- **cv2.THRESH_TOZERO** : L'intensité des pixels est fixée à 0, pour toutes les intensités de pixels, inférieure à la valeur seuil.
## Fonction

Retourne la valeur du seuil utilisée et l'image transformée.

## Vigilances & tips

Un seuil global convient surtout lorsque l'éclairage est relativement homogène.
rajouter un cmap = gray où l'image sera en jaune et violet
[Documentation officielle](https://docs.opencv.org/4.x/d7/d4d/tutorial_py_thresholding.html)
