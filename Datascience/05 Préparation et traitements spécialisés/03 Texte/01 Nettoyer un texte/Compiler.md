
# Compiler 

## Pattern

```python
import re

txt = "J'habite au 137 bd Auguste Blanqui, au 12ème étage"

r=re.compile(r"[0-9]+") #Détecte les nombre dans la chaine de caractère
p1 = r.findall(txt)
print(p1)

r=re.compile(r"[0-9]{3,}") #Détecte les nombre d'au moins 3 chiffres
p2 = r.findall(txt)
print(p2)

r=re.compile(r"\w+ment") #identifie les adverbes en -ment 

r=re.compile(r"https?://.[^\s]+") #pour capturer les liens html

r=re.compile(r"[a-zA-Z0-9é._-]+@\w+\.\w+") #pour capturer les adresses mails


r1 = re.findall(r"s.*o", 'stackoverflow') #sortie greddy stackoverflo
r2 = re.findall(r"s.*?o", 'stackoverflow') # sortie lazy stacko
```


![[Pasted image 20261013105611.png]]