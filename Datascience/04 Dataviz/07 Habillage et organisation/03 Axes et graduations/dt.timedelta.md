# `dt.timedelta`

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

Ajouter une durée à une date :

```python
import datetime as dt

date_nouvelle = dt.datetime(2016, 1, 1) + dt.timedelta(days=30)
```

Créer une séquence de dates espacées d'un même nombre de jours :

```python
dates = [
    dt.datetime(2016, 1, 1) + dt.timedelta(days=30 * i)
    for i in range(12)
]
```

````python
### Générer une séquence de dates

Pour construire rapidement des dates destinées à un axe temporel :

```python
dates = [
    dt.datetime(2016, 1, 1) + dt.timedelta(days=30 * i)
    for i in range(12)
]


import datetime as dt
fig = plt.figure( figsize = (12, 4))
ax1 = fig.add_subplot(111)

dates = [dt.datetime(2016,1,1) + dt.timedelta(days=30*i) for i in range(12)]

values = np.random.rand(len(dates))

ax1.plot_date(dates, values, linestyle='-');
````
## Paramètres

- `dt.datetime(2016, 1, 1)` : crée la date de départ ; les trois arguments représentent respectivement l'**année**, le **mois** et le **jour**.
- `dt.timedelta(...)` : crée une durée à ajouter ou soustraire à une date.
- `days=30` : durée exprimée en jours. `timedelta` peut également représenter notamment des heures ou des secondes.
- `i` : numéro de l'itération dans la compréhension de liste.
- `days=30 * i` : augmente progressivement le décalage depuis la date de départ : 0 jour, 30 jours, 60 jours, etc.
- `range(12)` : produit 12 décalages, de `i = 0` à `i = 11`.

## Fonction

`dt.timedelta` représente une **durée**. Elle permet notamment d'ajouter ou de soustraire un nombre de jours, d'heures ou de secondes à une date.

## Vigilances & tips

`timedelta(days=30)` signifie **30 jours**, pas « un mois ». Les mois n'ayant pas tous la même durée, une séquence par pas de 30 jours ne correspond pas exactement aux débuts de mois.

**S'utilise souvent avec :** `dt.datetime()`, `range()`
**`dt.timedelta` n'est pas une commande Matplotlib.** Elle appartient au module Python `datetime`. Matplotlib ne fait ici que consommer les dates que l'on a créées.
