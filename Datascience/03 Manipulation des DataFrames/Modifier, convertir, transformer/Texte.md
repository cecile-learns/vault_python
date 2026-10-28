# Texte

## Syntaxe

```python
texte_nettoye = df['col_texte'].str.strip()
texte_minuscules = df['col_texte'].str.lower()
texte_majuscules = df['col_texte'].str.upper()
elements_separes = df['col_texte'].str.split('separateur')
```

## Paramètres

- `'separateur'` : chaîne utilisée pour découper chaque texte. Sans argument, `str.split()` découpe sur les espaces successifs.

## Fonction

- `str.strip()` : retire les espaces au début et à la fin.
- `str.lower()` : convertit le texte en minuscules.
- `str.upper()` : convertit le texte en majuscules.
- `str.split()` : découpe chaque chaîne en liste d’éléments.
