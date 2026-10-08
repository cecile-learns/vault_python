
# `str.fullmatch()`

## Syntaxe

Vérifier que **toute la chaîne** respecte un motif :

```
masque = df["col_texte"].str.fullmatch(    r"motif_regex",    na=False)
```

Usage fréquent après normalisation de la colonne :

```
serie_texte = df["col_texte"].astype("string").str.strip()masque = serie_texte.str.fullmatch(    r"\d+[A-Za-z]*",    na=False)df_non_conformes = df.loc[~masque]
```

## Paramètres

- `df["col_texte"]` : `Series` contenant les chaînes à vérifier.
- `r"motif_regex"` : expression régulière décrivant la structure attendue.
- `r` : indique une chaîne _raw_ ; pratique avec les expressions régulières pour écrire directement `\d`, `\s`, etc.
- `\d+` : un ou plusieurs (`+`) chiffres (`\d`).
- `[A-Za-z]*` : zéro, une ou plusieurs (`*`) lettres majuscules ou minuscules.
- `na=False` : les valeurs manquantes sont considérées comme ne correspondant pas au motif.

Le résultat est une `Series` booléenne :

```
True     → toute la chaîne respecte le motif
False    → la chaîne ne respecte pas le motif
```

## Exemple

```
stock = df["StockCode"].astype("string").str.strip()masque = stock.str.fullmatch(r"\d+[A-Za-z]*", na=False)df_non_conformes = df.loc[    ~masque,    ["StockCode", "Description"]]
```

## Fonction

`str.fullmatch()` vérifie si **l'intégralité de chaque chaîne** d'une `Series` respecte une expression régulière. Elle retourne un masque booléen utilisable pour filtrer le DataFrame.

## Vigilances & tips

`fullmatch()` exige que **toute la chaîne** corresponde au motif : ce n'est pas simplement une recherche d'un motif quelque part dans le texte.

`~masque` inverse les booléens : il permet donc ici de récupérer les valeurs **non conformes**.

Nettoyer les espaces avec `.str.strip()` avant le contrôle évite qu'un espace parasite fasse échouer une valeur autrement conforme.

**S'utilise souvent avec :** `astype()`, `str.strip()`, `loc[]`