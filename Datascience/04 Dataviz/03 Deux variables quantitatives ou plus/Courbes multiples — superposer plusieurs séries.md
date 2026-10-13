# Courbes multiples — superposer plusieurs séries

> [!info] Bibliothèque
> Matplotlib

## Pour faire quoi ?

Tracer plusieurs courbes dans un même graphique, avec une couleur, un style et une légende par série. Les abscisses peuvent représenter autre chose que le temps.

## Script réutilisable

```python
import matplotlib.pyplot as plt

valeurs_x = [50, 100, 150, 200]
valeurs_y_1 = [2, 3, 7, 10]
valeurs_y_2 = [2, 7, 9, 10]

plt.grid(True)
courbe_1 = plt.plot(valeurs_x, valeurs_y_1, 'b-*',
                    linewidth=0.8, label='Trajet 1')
courbe_2 = plt.plot(valeurs_x, valeurs_y_2, 'g-+',
                    linewidth=0.8, label='Trajet 2')
plt.xlabel('Vitesse')
plt.ylabel('Temps')
plt.legend()
plt.show()
```

## À adapter

- `valeurs_x`, `valeurs_y_1`, `valeurs_y_2` : listes, tableaux NumPy ou colonnes Pandas. Remplacer les valeurs d’exemple par tes données.
- `label` : nom de chaque série dans la légende ; textes des axes : variables et unités.
- `'b-*'` : bleu (`b`), ligne continue (`-`), marqueur étoile (`*`).
- `'g-+'` : vert (`g`), ligne continue (`-`), marqueur plus (`+`).
- `linewidth` : épaisseur du trait en points ; chaque appel à `plot` retourne une liste de lignes.
- Ajouter un appel à `plt.plot()` pour chaque série supplémentaire.

### Variante rencontrée — un seul appel

```python
import matplotlib.pyplot as plt
import numpy as np

#exemple du cours
t=np.arange(0,5,0.2)

plt.plot(t,t,t,t**2,t,t**3)
plt.ylim([0,50]);


#ou plus simple visuellement pour éviter les pb
plt.plot(t,t,':r')
plt.plot(t,t**2,'-g')
plt.plot(t,t**3,'--b')

plt.plot(t,t,'hy' )
plt.plot(t,t**2,'-g', linewidth=5)
plt.plot(t,t**3,'-bD')
plt.show();

```

Les arguments se répètent par blocs **abscisses, ordonnées, format**. Ici, `np.arange` produit des abscisses de 0 inclus à 5 exclu par pas de 0,2 ; `**2` et `**3` calculent le carré et le cube. Les formats donnent des pointillés rouges, un trait continu vert et des tirets bleus. `ylim` fixe la plage affichée en ordonnée ; `courbes` contient les trois lignes.

### Variante — deux colonnes d’un DataFrame

```python
import matplotlib.pyplot as plt

col_x = 'vitesse'
col_serie_1 = 'mesure'
col_serie_2 = 'objectif'
plt.plot(df[col_x], df[col_serie_1], marker='o', linestyle='--',
         color='green', label=col_serie_1)
plt.plot(df[col_x], df[col_serie_2], linestyle='-',
         color='red', label=col_serie_2)
plt.title('Courbes comparées')
plt.xlabel(col_x)
plt.ylabel('Valeur')
plt.legend()
plt.show()
```

`df` est déjà chargé ; `col_x` donne les abscisses et `col_serie_1`, `col_serie_2` les deux séries. Remplacer les noms de colonnes et préciser les unités des axes. Si x représente des dates, préparer leur type et leur ordre chronologique.

## Vigilances & tips

- Plusieurs appels successifs à `plot` superposent les courbes sur le graphique courant. Ne pas créer une nouvelle figure entre ces appels si tu veux conserver la superposition.
- Chaque paire x/y doit avoir la même longueur. Des séries différentes peuvent avoir leurs propres abscisses : `plt.plot(x_1, y_1)` puis `plt.plot(x_2, y_2)`.
- Les points sont reliés dans l’ordre fourni. Pour représenter une fonction ou une évolution, ordonner les abscisses au préalable.
- Pour des styles, épaisseurs et labels différents selon la série, les appels séparés sont les plus faciles à adapter. Dans un appel unique, les paramètres nommés comme `linewidth` s’appliquent à toutes les courbes.
- La chaîne compacte est un argument positionnel de `plt.plot`, pas un paramètre `style=`. Ce dernier apparaît dans la variante Pandas `df.plot` du notebook.
- Les séries partagent ici une même échelle en ordonnée : vérifier que les unités sont comparables.

## Commandes utilisées

`plt.plot()`, `plt.grid()`, `plt.xlabel()`, `plt.ylabel()`, `plt.legend()`, `plt.ylim()`, `plt.show()` ; `np.arange()` pour la variante de fonctions.

Source : Liora — Courbes multiples, styles de ligne et légendes. Scripts issus des séquences du notebook, avec noms de variables explicites et affichage final.
