# Écrire un motif — expressions régulières

## Syntaxe

```python
import re

motif = re.compile(r"[A-Za-zÀ-ÿ']+")
```

## Repères

- `[...]` : classe de caractères ;
- `*`, `+`, `?`, `{n,m}` : quantificateurs ;
- `(...)` : groupe de capture ;
- `^` et `$` : début et fin ;
- `\d`, `\w`, `\s` : chiffre, caractère de mot, espace ;
- `.*?` : répétition non gourmande.

## Version lisible

```python
motif_url = re.compile(
    r"""
    https?://
    [A-Za-z0-9.\-/]+
    """,
    re.VERBOSE,
)
```

## Vigilances & tips

Utiliser une chaîne brute `r"..."` pour limiter les conflits avec les antislashs Python.

Documentation : [module `re`](https://docs.python.org/3/library/re.html)
