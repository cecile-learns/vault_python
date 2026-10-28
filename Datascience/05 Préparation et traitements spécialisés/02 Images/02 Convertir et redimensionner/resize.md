# `cv2.resize`

## Syntaxe

```python
image_reduite = cv2.resize(
    image_source,
    dsize=(75, 75),
    interpolation=cv2.INTER_AREA,
)


fig = plt.figure(figsize = (12,12))

fig.add_subplot(1,2,1)
plt.imshow(cv2.resize(img_color, dsize = (75,75),interpolation = cv2.INTER_AREA ))
plt.xticks([])
plt.yticks([])

fig.add_subplot(1,2,2)
plt.imshow(cv2.resize(img_color, dsize = (1000,1000)))
plt.xticks([])
plt.yticks([])

plt.show();
```
![[Pasted image 20261023092858.png]]

```python
#zoomer
long, larg = img_color.shape[:2]
long_z = int(0.5*long)
larg_z = int(0.5*larg)
long_deb = 0
larg_deb = 0
img_zoom = img_color[long_deb:long_deb + long_z, larg_deb:larg_deb + larg_z, :]

plt.figure(figsize = (8,5))

plt.imshow(img_zoom)
plt.xticks([])
plt.yticks([])

plt.show();
```
![[Pasted image 20261023145528.png]]
## Paramètres

- `image_source` : tableau contenant l'image ;
- `dsize=(largeur, hauteur)` : dimensions de sortie ;
- `interpolation` : méthode de calcul des nouveaux pixels ; `cv2.INTER_AREA` est adaptée à une réduction.

## Fonction

Retourne une image redimensionnée.

## Vigilances & tips

La taille est donnée dans l'ordre `(largeur, hauteur)`, contrairement à `shape`.

[Documentation officielle](https://docs.opencv.org/4.x/da/d54/group__imgproc__transform.html)
