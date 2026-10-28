# cat, head et tail

## Pour faire quoi ?

Lire le contenu complet ou une partie d’un fichier texte.

## Arguments

Le chemin du fichier est l’argument. Plusieurs fichiers peuvent être donnés à `cat`.

## Options ou éléments utiles

`head -n 5` : cinq premières lignes ; `tail -n 5` : cinq dernières ; `tail -f` : suit les nouvelles lignes ajoutées.

## Exemple du cours

```bash
head -n 10 fichier.txt
tail -n 10 fichier.txt
```

## Vigilances et repères

`cat` est pratique pour un petit fichier. Pour un gros fichier, préférer `head`, `tail` ou un outil de lecture paginée.
