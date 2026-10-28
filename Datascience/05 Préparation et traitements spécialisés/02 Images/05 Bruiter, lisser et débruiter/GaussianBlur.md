# `cv2.GaussianBlur`

## Syntaxe

```python
image_filtree = cv2.GaussianBlur(
    image_source,
    ksize=(3, 3),
    sigmaX=0,
)
plt.figure(figsize = (8,5))
plt.imshow(image_filtree)
plt.xticks([])
plt.yticks([])

plt.show();
```
![[Pasted image 20261023152135.png]]
## Paramètres

- image source ;
- `ksize` : dimensions du noyau gaussien, positives et impaires ;
- `sigmaX` : écart-type horizontal ; avec `0`, OpenCV le déduit de la taille du noyau.

## Fonction

Lisse l'image avec un noyau gaussien, notamment avant un seuillage ou une détection de contours.

[Documentation officielle](https://docs.opencv.org/4.x/d4/d86/group__imgproc__filter.html)
