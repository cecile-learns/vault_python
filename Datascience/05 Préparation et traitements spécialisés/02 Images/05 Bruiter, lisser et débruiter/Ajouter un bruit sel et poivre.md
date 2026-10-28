# Ajouter un bruit sel et poivre

## Pour faire quoi ?

Créer artificiellement des pixels noirs et blancs pour observer le comportement des filtres.

## Script réutilisable

```python
import numpy as np

valeur_sel = 20
bruit = np.random.randint(
    valeur_sel + 1,
    size=image_source.shape[:2],
)

image_bruitee = image_source.copy()
image_bruitee[np.where(bruit == 0)] = 0
image_bruitee[np.where(bruit == valeur_sel)] = 255
```

## À adapter

- `valeur_sel` : plus elle est grande, plus les pixels modifiés sont rares ;
- valeurs `0` et `255` selon l'échelle de l'image.

## Vigilances & tips

Travailler sur une copie pour conserver l'image originale.

## Commandes utilisées

`np.random.randint`, `shape`, `copy`, `np.where`.
