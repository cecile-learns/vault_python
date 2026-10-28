# str.contains

## Syntaxe

```python
lignes_contenant_mot = df[df['col_texte'].str.contains('mot_recherche', case=False, na=False)]
```

## Paramètres

`'mot_recherche'` : texte ou motif recherché.

`case=False` : ignore la casse.

`na=False` : considère les valeurs manquantes comme non correspondantes.

## Fonction

Filtre les lignes selon la présence d’un texte dans une colonne.
