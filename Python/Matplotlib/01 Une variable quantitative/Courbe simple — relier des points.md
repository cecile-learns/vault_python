# Courbe simple — relier des points

## Pour faire quoi ?

Représenter une série quantitative par une courbe. Les abscisses peuvent être les positions des observations ou des valeurs explicites : vitesse, distance, temps, etc.

## Script réutilisable

```python
import matplotlib.pyplot as plt

valeurs_y = [1, 3, 2, 4]
courbe = plt.plot(valeurs_y)
plt.show()


```

## À adapter

- `valeurs_y` : liste, tableau NumPy ou colonne numérique, par exemple `df['montant']`.
- Avec une seule série fournie, les abscisses sont les positions 0, 1, 2…

### Variante — abscisses explicites

```python
import matplotlib.pyplot as plt

col_x = 'vitesse'
col_y = 'mesure'
courbe = plt.plot(df[col_x], df[col_y], marker='o', linestyle='-', label=col_y)
plt.title(f'{col_y} selon {col_x}')
plt.xlabel(col_x)
plt.ylabel(col_y)
plt.legend()
plt.show()


#exemple cours 
x='Mois'
y='Produit 1'


plt.plot(df['Month'],df['Product1'])

plt.xlabel(x)
plt.ylabel(y)
plt.title(f"{y} en fonction de {x}");

#Tout peut se résumer à 
df.plot(x = 'Month', y='Product1', title= "Ventes par mois");
```

- `df` : DataFrame chargé ; `col_x`, `col_y` : colonnes de coordonnées.
- `marker='o'` : cercles ; `linestyle='-'` : trait continu ; `label` : nom de la série dans la légende. `courbe` contient la liste des lignes retournées.

## Vigilances & tips

- `plot` relie les points dans l’ordre fourni : cet ordre doit avoir un sens.
- Avec des abscisses explicites, chaque x doit correspondre à un y. Pour représenter une évolution selon x, trier les données selon x au préalable.
- Les positions automatiques ne représentent pas des distances ou des durées réelles.
- Le classement sous « Une variable quantitative » correspond à une série à tracer ; la variante avec x explicite peut mobiliser deux colonnes quantitatives.
- Pour des dates : préparer leur type et leur ordre chronologique. `plot` n’agrège pas plusieurs observations d’une même date.

## Commandes utilisées

`plt.plot()`, `plt.title()`, `plt.xlabel()`, `plt.ylabel()`, `plt.legend()`, `plt.show()`.

Sources : Liora — Introduction à la visualisation de données avec Matplotlib ; Data Visualisation, §2.5.
