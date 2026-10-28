# `cv2.Laplacian`

## Syntaxe

```python
laplacien = cv2.Laplacian(image_gris, ddepth=cv2.CV_64F)


laplacien = cv2.Laplacian(street_gray, ddepth=cv2.CV_64F)
plt.imshow(laplacien, cmap = 'gray')
plt.xticks([])
plt.yticks([])
plt.show();
```
![[Pasted image 20261023160149.png]]
## Paramètres

- image source ;
- `ddepth` : profondeur du tableau de sortie.

## Fonction

Calcule le Laplacien de l'image pour faire ressortir les changements rapides d'intensité dans toutes les directions.

[Documentation officielle](https://docs.opencv.org/4.x/d4/d86/group__imgproc__filter.html)
