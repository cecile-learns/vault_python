# `cv2.dilate`

## Syntaxe

```python
noyau = np.ones((5, 5), dtype=np.uint8)
image_dilatee = cv2.dilate(image_binaire, noyau)


noyau = np.ones((5, 5), dtype=np.uint8)
image_dilatee = cv2.dilate(erosion_2, noyau)

plt.imshow(image_dilatee, cmap = 'gray')
plt.xticks([])
plt.yticks([])

plt.show();
```
![[Pasted image 20261023162105.png]]

```python
diff= spider_gray - image_dilatee

plt.imshow(diff, cmap = 'gray')
plt.xticks([])
plt.yticks([])

plt.show();
```

![[Pasted image 20261023162125.png]]
## Paramètres

- image source ;
- noyau : tableau définissant le voisinage utilisé.

## Fonction

Agrandit les régions claires d'une image binaire.

## Vigilances & tips

La dilatation peut être appliquée après une érosion pour restaurer la taille des formes tout en supprimant certains petits détails.

[Documentation officielle](https://docs.opencv.org/4.x/d9/d61/tutorial_py_morphological_ops.html)
