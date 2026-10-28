# Découper et remplacer — `re.split` et `re.sub`

## Syntaxe

### split : Découper

```python
import re

#Compilateur
r = re.compile(r"\W+")

#Exemple
txt = "L'exemple... parfait pour comprendre"

#Split
print(r.split(txt))  #['L', 'exemple', 'parfait', 'pour', 'comprendre']


```


### sub : Remplacer
```python
txt = "c'est super cool comme superstition"

r = re.compile(r"super")
print(r.sub('cool', txt)) #c'est cool cool comme coolstition

r = re.compile(r"\bsuper\b") #pour éviter le chevauchement
print(r.sub('cool', txt)) # c'est cool cool comme superstition


```
## Paramètres

- le motif indique ce qui déclenche la séparation ou le remplacement ;
- le deuxième argument de `sub()` est la valeur de remplacement ;
- le dernier argument est le texte traité.

## Vigilances & tips

Un groupe capturant dans le motif de `split()` peut apparaître dans le résultat.

Documentation : [module `re`](https://docs.python.org/3/library/re.html)
