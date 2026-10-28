# `cv2.Sobel`

## Syntaxe

```python
bords_verticaux = cv2.Sobel(
    image_gris,
    ddepth=cv2.CV_64F,
    dx=1,
    dy=0,
)


street_gray = cv2.imread('street.jpg', cv2.IMREAD_GRAYSCALE)

bords_verticaux = cv2.Sobel(
    street_gray,
    ddepth=cv2.CV_64F,
    dx=1,
    dy=0,
)

plt.imshow(bords_verticaux, cmap = 'gray')
plt.xticks([])
plt.yticks([])

plt.show();
```
![[Pasted image 20261023160002.png]]
## Paramètres

- image source ;
- `ddepth=cv2.CV_64F` : profondeur du résultat, conservant les dérivées négatives ;
- `dx` : ordre de dérivation selon x ;
- `dy` : ordre de dérivation selon y.

## Fonction

Calcule une dérivée spatiale pour faire ressortir des bords selon une direction.

## Vigilances & tips

`dx=1, dy=0` met en évidence les variations horizontales de luminosité et donc les bords verticaux.

[Documentation officielle](https://docs.opencv.org/4.x/d4/d86/group__imgproc__filter.html)
