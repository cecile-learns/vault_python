# map

## Syntaxe

```python
libelles_statut = df['col_code'].map({
    'A': 'Actif',
    'I': 'Inactif'
})

valeurs_transformees = df['col_valeur'].map(fonction_transformation)
```

## Paramètres

- le dictionnaire associe chaque valeur recherchée à sa valeur de remplacement ;
- `fonction_transformation` est appelée pour chaque valeur de la Series.

## Fonction

Transforme chaque valeur d’une Series à partir d’un dictionnaire de correspondance ou d’une fonction.

## Vigilances & tips

Avec un dictionnaire, les valeurs absentes des clés deviennent généralement `NaN`.

**S'utilise souvent avec :** `replace()`, `apply()`.

```python
from dico import *

from dico import *

# Regroupement des appellations
df['product_variation_size_id'] = df['product_variation_size_id'].map(dico_size)
df['product_color'] = df['product_color'].map(dico_color)

# Gestion des valeurs manquantes
df['product_variation_size_id'] = df['product_variation_size_id'].fillna('other')
df['product_color'] = df['product_color'].fillna('other')
```

