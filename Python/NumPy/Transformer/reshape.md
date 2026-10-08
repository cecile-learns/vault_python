# reshape

## Syntaxe

```python
tableau_2d = tableau.reshape((lignes, colonnes))
```

## Fonction

Change la forme sans changer le nombre d’éléments.

## Vigilances & tips

Le produit des dimensions doit rester identique.


1 image réelle       → 8 × 8
1 observation pour ML → 64 variables

1797 images
↓ aplatissement
X.shape = (1797, 64)
           ↑      ↑
      observations variables


Pour réafficher les images, on fait l'opération inverse :
```
X_images = X.reshape(1797, 8, 8)
```
« les estimateurs classiques attendent généralement `X` sous la forme `(n_samples, n_features)`, et non un tenseur `(n_samples, hauteur, largeur)` »
