# str.replace

## Syntaxe

```python
texte_corrige = df['col_texte'].str.replace('texte_ancien', 'texte_nouveau', regex=False)
```

## Paramètres

Les deux premiers arguments sont le texte recherché et son remplacement.

`regex=False` : traite le texte recherché littéralement.

## Fonction

Remplace une sous-chaîne dans chaque valeur textuelle.
