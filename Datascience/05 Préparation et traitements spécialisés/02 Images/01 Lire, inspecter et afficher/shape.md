# `shape`

## Syntaxe

```python
hauteur, largeur, canaux = image_couleur.shape
hauteur, largeur = image_gris.shape
```

## Paramètres

`shape` ne prend aucun paramètre. Il retourne les dimensions du tableau.

## Fonction

Permet de connaître la hauteur, la largeur et, pour une image couleur, le nombre de canaux.

## Vigilances & tips

L'ordre est `(hauteur, largeur, canaux)`. Les fonctions OpenCV qui demandent une taille utilisent souvent `(largeur, hauteur)`.

[Documentation NumPy](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.shape.html)
