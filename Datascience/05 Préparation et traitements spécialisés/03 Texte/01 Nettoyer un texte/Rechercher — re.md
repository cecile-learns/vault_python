
## Fonction

- `findall()` renvoie les correspondances ;
- `finditer()` les parcourt sans construire immédiatement toute la liste ;
- `search()` cherche partout ;
- `match()` exige une correspondance au début.

Un objet Match donne accès à `group()` et `groups()`.

## Findall et finditer
```python
txt = "findall et finditer"
reg = re.compile("([a-zA-Zàéè\']+)") # groupe de capture de mots

# Utilisation de findall
mots = reg.findall(txt)
# Tous les mots sont stockés dans la mémoire
print('Pour la méthode findall(), on a :', mots) 
#Pour la méthode findall(), on a : ['findall', 'et', 'finditer']

print('Les mots trouvé sont donc :')
for mot in mots:
    print(mot)


# Utilisation de finditer
matches = reg.finditer(txt)
# Les mots sont chargés dans la mémoire sur demande
print('\nPour la méthode finditer(), on a :',matches)
#Pour la méthode finditer(), on a : <callable_iterator object at 0x7f8999130520>

print('Les mots trouvé sont donc :')
for match in matches:
    # match est un objet de type Match
    print(match.group(1))


```


## Match
```python
r = re.compile('(a(b)c)d') # groupe 1 : abc, groupe 2: abcd et le sans notion de groupe abcd
g = r.match('abcd')

#Utilisation de group
#.group(0) renvoie la chaîne entière détectée par l'expression
print('group(0) renvoie', g.group(0)) #abcd

# .group(1) renvoie le premier groupe capturé
print('group(1) renvoie', g.group(1)) #abc

# .group(2) renvoie le deuxième groupe capturé
print('group(2) renvoie', g.group(2)) #b


#Utilisation de groups
# .groups() renvoie tous les groupes capturés
print('groups() renvoie', g.groups()) #'abc" et 'b'
```

## Search
```python
txt = 'https://www.google.com/ \n http://www.safari.fr/data-science'

r = re.compile(r"http(s)?://[a-zA-z0-9\.\-/]+")
liens=r.search(txt)

print(liens.group())  
# affiche la première correspondance : https://www.google.com/)

print(liens.groups()) 
# affiche les sous chaine détectées : 's'
```




Documentation : [module `re`](https://docs.python.org/3/library/re.html)

