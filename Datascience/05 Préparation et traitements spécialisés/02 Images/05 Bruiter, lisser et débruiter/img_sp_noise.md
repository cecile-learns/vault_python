
# img_sp_noise

## Syntaxe

```python
salt_value = 20

noise = np.random.randint(salt_value+1, size=(150, 150))
img_sp_noise = img_color.copy()

#---------- Poivre---#

indexe = np.where(noise == 0)

A = indexe[0]
B = indexe[1]

img_sp_noise[A,B,:] = 0

#---------- Sel---------#

indexe = np.where(noise == salt_value)

A = indexe[0]
B = indexe[1]

img_sp_noise[A,B,:] = 255

plt.figure(figsize = (8,5))

plt.imshow(img_sp_noise)
plt.xticks([])
plt.yticks([])

plt.show();
```
![[Pasted image 20261023151558.png]]
## Paramètres

- 

## Fonction

Avoir une image contenant aléatoirement des pixels blanc ou noir

## Vigilances & tips

Un noyau plus grand lisse davantage et efface plus de détails.

[Documentation officielle](https://docs.opencv.org/4.x/d4/d86/group__imgproc__filter.html)
