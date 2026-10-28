# `cv2.imread`  / `plt.imshow` 

## Syntaxe

```python
image_color = cv2.imread("chemin/image.jpg", cv2.IMREAD_COLOR)
img_color = cv2.cvtColor(img_color, cv2.COLOR_BGR2RGB)

image_gris = cv2.imread("chemin/image.jpg", cv2.IMREAD_GRAYSCALE)


plt.figure(figsize = (8,5))
plt.imshow(img_gray, cmap = 'gray')
plt.xticks([])
plt.yticks([])
plt.show();



plt.figure(figsize = (8,5))
plt.imshow(img_color)
plt.xticks([])
plt.yticks([])

plt.show();
```

## Paramètres

- chemin : emplacement du fichier image ;
- `cv2.IMREAD_COLOR` : charge une image couleur en BGR ;
- `cv2.IMREAD_GRAYSCALE` : charge directement une image à un canal.
- 

## Fonction

Charge une image dans un tableau NumPy. Renvoie `None` si le fichier n'a pas pu être lu.
Afficher l'image

## Vigilances & tips

```python
if image_couleur is None:
    raise FileNotFoundError("Image introuvable ou illisible")
```

Une image couleur OpenCV est chargée en BGR, pas en RGB.

[Documentation officielle](https://docs.opencv.org/4.x/d4/da8/group__imgcodecs.html)
