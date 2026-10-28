# `cv2.line`

## Syntaxe

```python
cv2.line(
    image_lignes,
    (x1, y1),
    (x2, y2),
    color=(255, 0, 0),
    thickness=3,
)


#cours 
line_img = np.zeros((building_color.shape[0],building_color.shape[1],3), dtype=np.uint8)
for line in lines:
    for x1, y1, x2, y2 in line:
        cv2.line(line_img, (x1, y1), (x2, y2), color = [255, 0, 0], thickness=3)

building_lines = cv2.addWeighted(building_color, 0.8, line_img, 1.0, 0.0)

plt.figure(figsize = (8,5))
plt.imshow(building_lines)
plt.xticks([])
plt.yticks([])
plt.show();
```
![[Pasted image 20261023155624.png]]
## Paramètres

- image à modifier ;
- coordonnées des deux extrémités ;
- `color` : couleur du segment dans l'espace colorimétrique de l'image ;
- `thickness` : épaisseur en pixels.

## Fonction

Dessine directement un segment dans le tableau image fourni.

## Vigilances & tips

La fonction modifie l'image passée en argument.

[Documentation officielle](https://docs.opencv.org/4.x/d6/d6e/group__imgproc__draw.html)
