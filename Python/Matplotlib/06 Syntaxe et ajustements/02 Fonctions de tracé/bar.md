# bar

## Syntaxe

```python
barres = plt.bar(categories, valeurs, width=0.6, color="blue",
                 edgecolor="black", label="Total")
                 
#exemple de cours
plt.bar([1,3,5,7,9],[5,2,7,8,2],color='k', label='Exemple 1');
plt.bar([2,4,6,8,10],[8,6,2,5,6],color='r', label='Exemple 2');

plt.xlabel('Numéros')
plt.ylabel('Hauteur')

plt.legend()
plt.title('Mon graphique en barre')

plt.show()
                 
                 
```


## Paramètres

`categories` : positions ou noms ; `valeurs` : hauteurs ; `width` : largeur ; `color`, `edgecolor` : intérieur et contour ; `label` : légende.

## Fonction

Trace des barres verticales à partir de valeurs déjà calculées.

## Vigilances & tips

Empiler : second appel avec `bottom=valeurs_premiere_serie`. 
```python
plt.bar(range(len(df)), df.Product1, label = "Produit1")
plt.bar(range(len(df)), df.Product2, bottom = df.Product1, label = "Produit2")

plt.legend();

#Depuis un dataFrame

df.plot.bar(
    x='Month',
    y=['Product1', 'Product2'],
    stacked=True
)

```

Côte à côte : décaler les positions de la deuxième série de `width`. 
```python
barWidth=0.4
x1=range(12)
x2=[r + barWidth for r in x1]

df12 = df.head(12)

plt.bar(x1, df12.Product1, width = barWidth, label = "Produit1")
plt.bar(x2, df12.Product2, width = barWidth, label = "Produit2")

plt.xticks([0,2,4,6,8,11], ['Janvier', 'Mars', 'Mai', 'Juillet', 'Septembre', 'Decembre'])

plt.legend()
plt.show()

#Depuis un dataFrame
df.head(6).plot.bar(x = 'Month', y=['Product1', 'Product2', 'Returns'],stacked=True,
                     rot=0)
plt.xticks(range(6), ["Janvier","Février","Mars","Avril","Mai","Juin"])
plt.legend();
```
Autres paramètres rencontrés : `yerr` (amplitudes des erreurs), `ecolor` (leur couleur), `align` (centre ou bord), `hatch` (hachures), `linewidth` (contour).

*Attention pour que le label s'affiche notamment quand il y a plusieurs diagramme, il faut ajouter un plt.legend()*

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.


## Mémo — contours, barres d’erreur et hachures

### `edgecolor` — couleur du contour des barres

| Écriture | Effet |
|---|---|
| `edgecolor='black'` ou `'k'` | Contour noir |
| `edgecolor='red'` ou `'r'` | Contour rouge |
| `edgecolor='white'` ou `'w'` | Contour blanc |
| `edgecolor='#367d7d'` | Couleur définie par un code hexadécimal |
| `edgecolor=['black', 'red', 'blue']` | Un contour différent pour chacune des trois barres |

`color` règle le remplissage ; `edgecolor` règle le contour. `linewidth=1.5` règle l’épaisseur du contour en points.

### `ecolor` — couleur des barres d’erreur

| Écriture | Effet |
|---|---|
| `ecolor='black'` ou `'k'` | Barres d’erreur noires |
| `ecolor='red'` ou `'r'` | Barres d’erreur rouges |
| `ecolor='gray'` | Barres d’erreur grises |
| `ecolor='#367d7d'` | Couleur définie par un code hexadécimal |

`ecolor` ne crée pas de barres d’erreur : fournir `yerr` pour les dessiner. `yerr` contient des amplitudes non négatives autour des hauteurs : un nombre commun à toutes les barres, ou une valeur par barre pour des erreurs symétriques. Ces amplitudes sont à calculer selon ce que tu veux représenter ; Matplotlib ne choisit pas leur sens statistique.

### `hatch` — motif des hachures

| Écriture Python | Motif |
|---|---|
| `hatch='/'` | Diagonales montantes |
| `hatch='\\'` | Diagonales descendantes |
| `hatch='-'` | Lignes horizontales |
| `hatch='+'` | Quadrillage horizontal et vertical |
| `hatch='x'` | Diagonales croisées |
| `hatch='.'` | Points |
| `hatch='o'` | Petits cercles |
| `hatch='O'` | Grands cercles |
| `hatch='*'` | Étoiles |

Dans une chaîne Python, `\\` écrit un seul antislash. Répéter un motif augmente sa densité : `hatch='///'`. Choisir un contour contrasté aide à rendre les hachures visibles.

### Exemple combiné

```python
import matplotlib.pyplot as plt

categories = ['A', 'B', 'C']
valeurs = [12, 18, 15]
amplitudes_erreur = [1, 2, 1.5]

barres = plt.bar(categories, valeurs,
                 color='lightblue',
                 edgecolor='black', linewidth=1.5,
                 hatch='/',
                 yerr=amplitudes_erreur, ecolor='red')
plt.show()

plt.bar(range(7),[3,4,5,6,5,4,3], color='b', width=0.6, hatch='/');


```

Trois barres bleu clair, à contours noirs et hachures diagonales, avec des barres d’erreur rouges. Remplacer `categories`, `valeurs` et `amplitudes_erreur` par tes données ; les erreurs d’exemple sont fictives. `barres` contient les objets graphiques retournés.

### Documentation officielle

- [Matplotlib — paramètres de bar](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.bar.html)
- [Matplotlib — aperçu visuel des hachures](https://matplotlib.org/stable/gallery/shapes_and_collections/hatch_style_reference.html)
