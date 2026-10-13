# chmod

## Pour faire quoi ?

Modifier les droits de lecture, écriture et exécution d’un fichier ou dossier.

## Arguments

Premier argument : mode de droits ; second : fichier ou dossier ciblé.

## Options ou éléments utiles

Notation symbolique : `u` propriétaire, `g` groupe, `o` autres ; `+`, `-`, `=` ajoutent, retirent ou fixent ; `r`, `w`, `x`. Notation numérique courante : `755`, `644`. `-R` agit récursivement.

## Exemple du cours

```bash
chmod +x script.sh
chmod 644 notes.txt
```

## Vigilances et repères

`chmod +x` rend un script exécutable. Les droits numériques doivent être compris avant usage : `777` accorde tout à tout le monde.
