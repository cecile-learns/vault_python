# `cv2.erode`

## Syntaxe

```python
noyau = np.ones((5, 5), dtype=np.uint8)
image_erodee = cv2.erode(image_binaire, noyau)


spider_gray = cv2.imread('spider.jpeg', cv2.IMREAD_GRAYSCALE)
spider_gray = cv2.resize(spider_gray, (150,150))
spider_gray = 255 - spider_gray

filtre = cv2.GaussianBlur(spider_gray, ksize = (3,3), sigmaX = 0)

fig = plt.figure(figsize = (12,12))

fig.add_subplot(1,4,1)

plt.imshow(spider_gray, cmap = 'gray')
plt.xticks([])
plt.yticks([])

fig.add_subplot(1,4,2)

kernel_1 = np.ones((3,3),np.uint8)
erosion_1 = cv2.erode(filtre, kernel_1)

plt.imshow(erosion_1, cmap = 'gray')
plt.xticks([])
plt.yticks([])

fig.add_subplot(1,4,3)

kernel_2 = np.ones((5,5),np.uint8)
erosion_2 = cv2.erode(filtre, kernel_2)

plt.imshow(erosion_2, cmap = 'gray')
plt.xticks([])
plt.yticks([])

fig.add_subplot(1,4,4)

kernel_3 = np.ones((7,7),np.uint8)
erosion_3 = cv2.erode(filtre, kernel_3)

plt.imshow(erosion_3, cmap = 'gray')
plt.xticks([])
plt.yticks([])

plt.show();
```
![[Pasted image 20261023160745.png]]
## Paramètres

- image source ;
- noyau : tableau définissant le voisinage utilisé.

## Fonction

Réduit les régions claires d'une image binaire. Un noyau plus grand produit une érosion plus marquée.

## Vigilances & tips

Dans le cours, les formes d'intérêt sont placées en blanc avant l'opération.

[Documentation officielle](https://docs.opencv.org/4.x/d9/d61/tutorial_py_morphological_ops.html)
