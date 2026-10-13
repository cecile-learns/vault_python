# for

## Syntaxe

```python
for element in sequence:
    print(element)
```

## Fonction

Répète un bloc pour chaque élément d'une séquence.

## Vigilances & tips

`for` récupère la valeur ; utiliser `enumerate()` si l'indice est aussi nécessaire.


```python
for color in colors:
    for shape in shapes:
        print(color, shape)
```