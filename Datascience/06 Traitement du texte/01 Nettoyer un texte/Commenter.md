
# re.verbose 

## Syntaxe


```python
import re

txt = """
Contactez-nous à marie.dupont@gmail.com
ou à pierre-martin@entreprise.fr.
Adresse incorrecte : paul@entreprise
"""

# Construction du motif
r = re.compile(r"""
    [a-zA-Z0-9._-]+   # Nom avant le @ : lettres, chiffres, point, tiret, _
    @                 # Caractère @ obligatoire
    [a-zA-Z0-9-]+     # Nom de domaine
    \.                # Point obligatoire
    [a-zA-Z]{2,}      # Extension : au moins 2 lettres
""", re.VERBOSE)

# Recherche
emails = r.findall(txt)

print(emails)
# ['marie.dupont@gmail.com', 'pierre-martin@entreprise.fr']

```


## Paramètres

- 

## Vigilances & tips



Documentation : [module `re`](https://docs.python.org/3/library/re.html)
