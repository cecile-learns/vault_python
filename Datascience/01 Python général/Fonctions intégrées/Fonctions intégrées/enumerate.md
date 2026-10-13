# enumerate

## Syntaxe

```python
for index, valeur in enumerate(liste):
    print(index, valeur)
```

## Fonction

Parcourt simultanément les indices et les valeurs.

## Vigilances & tips

L'ordre retourné est toujours `index, valeur`.


```python
def rgb_to_gray(X):
    X_gray=np.zeros(shape=(32,32,1))
    for i, row in enumerate(X):
        for j, pixel in enumerate(row):
            X_gray[i,j]=np.mean(pixel)
    return X_gray  
```

je pars d'un array(32,32,3) et je veux arriver a un array(32,32,0)
pour cela je dois parcourir chaque pixel et faire la moyenne du R,G,B
ici row correspond à une ligne de l'image donc un array(32,3)
ensuite je cherche mon pixel qui contient mes trois composant
j'ai utilisé enumerate notamment parce que j'ai besoin d'un index associé à la valeur
donc je peux dire au niveau du pixel qui a pour coordonné i,j tu mets comme valeur la moyenne du rgb


