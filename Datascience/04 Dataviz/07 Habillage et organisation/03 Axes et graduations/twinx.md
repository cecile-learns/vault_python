
> [!info] Bibliothèque
> Matplotlib

# twinx

## Syntaxe

```python
x = np.arange(-10, 10, 0.1)
y1 = x**2
y2 = x**3
fig = plt.figure()
#
ax1 = fig.add_subplot(111)
ax1.plot(x, y1)
ax1.set_ylabel('x au carré')
#
ax2 = ax1.twinx()   #La Commande importante !
ax2.plot(x, y2, 'r')
ax2.set_ylabel('x au cube');
```
![[Pasted image 20260922090301.png]]
## Paramètres



## Fonction

Permet de tracer deux séries différentes sur le même graphique avec différentes graduations d'axes

## Paramètres courants complémentaires

`plt.xlim()` sans argument retourne les limites actuelles. `left` et `right` peuvent aussi être fournis séparément, par exemple `plt.xlim(left=0)`. Inverser leur ordre inverse l’axe.

## Documentation officielle

[Matplotlib — `pyplot.xlim`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.xlim.html)
