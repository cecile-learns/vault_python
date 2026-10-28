# `cv2.Canny`

## Syntaxe

```python
contours = cv2.Canny(image_filtree, 100, 200)

#exemple cours
building_color = cv2.imread('building.jpg', cv2.IMREAD_COLOR)
building_color = cv2.cvtColor(building_color, cv2.COLOR_BGR2RGB)

filtre = cv2.GaussianBlur(building_color,ksize=(3, 3),sigmaX=0)

contours = cv2.Canny(filtre, 100, 200)

plt.figure(figsize = (8,5))
plt.imshow(contours, cmap = 'gray')
plt.xticks([])
plt.yticks([])

plt.show();
```
![[Pasted image 20261023155257.png]]
## Paramètres

- image source, généralement en niveaux de gris et préalablement lissée ;
- `100` : seuil bas ;
- `200` : seuil haut.

## Fonction

Retourne une image binaire contenant les contours détectés.

## Vigilances & tips

Les seuils contrôlent la sensibilité. Le lissage préalable réduit les faux contours provoqués par le bruit.

[Documentation officielle](https://docs.opencv.org/4.x/da/d22/tutorial_py_canny.html)
