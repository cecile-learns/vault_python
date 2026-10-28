# Relation — nuage de points

> [!info] Bibliothèque
> Matplotlib

Pour faire quoi ?

Visualiser la relation entre **deux variables quantitatives**.

Chaque point représente un couple de valeurs `(x, y)`.

---

## Matplotlib — commande de base

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [0, 2, 3, 8, 15]

plt.scatter(x, y)
plt.show()
```

---

## Avec un DataFrame Pandas

```python
col_x = 'Product1'
col_y = 'Product2'

plt.scatter(df[col_x], df[col_y])

plt.xlabel(col_x)
plt.ylabel(col_y)
plt.title(f'{col_y} selon {col_x}')

plt.show()
#Exemple du cours avec la coloration des points en fonction d'une troisème variable et l'ajout d'une map couleur pour légender cette coloration
df.plot.scatter(x='Product1', y='Product2', c = 'Returns', s=30, cmap = 'viridis');
```

---

## Ajouter une troisième variable par la couleur

```python
plt.scatter(
    df['Product1'],
    df['Product2'],
    c=df['Returns']
)

plt.show()
```

Ici :

- `Product1` → axe des abscisses
- `Product2` → axe des ordonnées
- `Returns` → couleur des points

---

## Modifier la taille des points

Taille fixe :

```python
plt.scatter(
    df['Product1'],
    df['Product2'],
    s=30
)
```

La taille peut aussi varier pour chaque point si `s` reçoit une liste ou une série de même longueur que les données.

---

## Version Pandas

```python
df.plot.scatter(
    x='Product1',
    y='Product2',
    c='Returns',
    s=30,
    cmap='viridis'
)
```

Cette syntaxe est pratique lorsque toutes les variables viennent du même DataFrame.

---

## Paramètres utiles

```python
plt.scatter(
    x,
    y,
    c='red',      # couleur
    s=40,         # taille des points
    marker='o'    # forme des points
)
```

Pour les palettes de couleurs avec une variable numérique :

```python
cmap='viridis'
```

Consulter la documentation Matplotlib pour les autres palettes, couleurs et marqueurs.

---

## Vigilances

- `x` et `y` doivent avoir le même nombre de valeurs.
- Chaque ligne du DataFrame doit correspondre à un couple `(x, y)`.
- Un nuage de points permet d'observer une relation éventuelle entre deux variables quantitatives.
- Une absence de corrélation linéaire forte n'exclut pas une relation non linéaire.

---

## À ne pas confondre

Pour un nuage de points, préférer :

```python
plt.scatter(x, y)
```

plutôt que :

```python
plt.plot(x, y)
```

`plt.plot()` relie les points par défaut, alors que `plt.scatter()` représente directement les observations sous forme de points.
