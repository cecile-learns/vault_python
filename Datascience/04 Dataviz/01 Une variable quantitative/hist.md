# hist

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
hauteurs, bornes, barres = plt.hist(valeurs, bins=20, density=False,
                                   edgecolor="black", rwidth=0.8)
# Variante rencontrée :
# hauteurs, bornes, barres = ax.hist(valeurs, bins=20)
```

## Paramètres

`valeurs` : série numérique ; 
range=(minimum, maximum)  pas obligatoire si bins est une liste bornée
`bins` : nombre d’intervalles ou bornes explicites ; 
`density` : densité plutôt qu’effectifs ; density=True
`edgecolor` : contour ; 
`rwidth` : fraction de largeur occupée.

## Fonction

Calcule et trace un histogramme ; retourne hauteurs, bornes et objets graphiques.

## Vigilances & tips



Avec `density=True`, l’aire vaut 1 ; les hauteurs ne sont pas des probabilités par classe. 
`range=(minimum, maximum)` limite la plage ; le dernier intervalle inclut sa borne droite. 
Plusieurs séries : `plt.hist([serie_1, serie_2], bins=20, label=["A", "B"])` ; 
ajouter `histtype="barstacked"` pour empiler. 
`orientation="horizontal"` inverse l’orientation.
df.head()
Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.


```python
df=pd.read_csv('sales_data.csv')
df.head()

plt.hist(
    [df.Product1,df.Product2], 
         bins=[100,200,280,325,450,600,800], 
         color=['#0086ad','#97ebdb'], 
         label=['Product1', 'Product2'],
    histtype="barstacked",
    rwidth = 0.8
        )

plt.title('Histogramme 2 series superposées')
plt.xlabel('Ventes')
plt.ylabel('Effectifs')

plt.legend()
plt.show()


#Méthode avec pandas
df.plot.hist(y=['Product1', 'Product2'], 
bins = 7, 
rwidth = 0.8 , 
color= ['#0c4c83', '#830c4c'], 
alpha=0.5);

df.plot.hist(y=['Product1', 'Product2'], 
bins = 7, 
subplots=True, 
rwidth = 0.8 , 
color= ['#0c4c83', '#830c4c'], 
alpha=0.5);

df.hist(column=['Product1', 'Product2'], 
bins = 7, 
rwidth = 0.8 , 
color= ['#0c4c83']);

```

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `range=(minimum, maximum)` | Limite la plage prise en compte pour construire les classes. |
| `weights` | Donne un poids à chaque observation. |
| `color`, `edgecolor` | Couleurs du remplissage et du contour. |
| `label` | Texte associé à chaque série pour la légende. |
| `alpha` | Opacité entre `0` et `1`. |
| `histtype` | Forme de l’histogramme : `'bar'`, `'barstacked'`, `'step'` ou `'stepfilled'`. |
| `orientation` | Orientation `'vertical'` ou `'horizontal'`. |
| `stacked=True` | Empile plusieurs séries. |

## Documentation officielle

[Matplotlib — `pyplot.hist`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.hist.html)
