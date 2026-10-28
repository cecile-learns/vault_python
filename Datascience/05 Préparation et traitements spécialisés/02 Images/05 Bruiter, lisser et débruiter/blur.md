# `cv2.blur`

## Syntaxe

```python
image_lissee = cv2.blur(image_source, ksize=(3, 3))

plt.figure(figsize = (8,5))

plt.imshow(image_lissee)
plt.xticks([])
plt.yticks([])

plt.show();
```
![[Pasted image 20261023151444.png]]
## Paramètres

- `image_source` : image à filtrer ;
- `ksize=(largeur, hauteur)` : taille du noyau de moyenne.

## Fonction

Lisse l'image en remplaçant chaque pixel par la moyenne de son voisinage.

## Vigilances & tips

Un noyau plus grand lisse davantage et efface plus de détails.

[Documentation officielle](https://docs.opencv.org/4.x/d4/d86/group__imgproc__filter.html)
