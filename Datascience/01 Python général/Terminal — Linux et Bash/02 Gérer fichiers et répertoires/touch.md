# touch

## Pour faire quoi ?

Créer un fichier vide, ou mettre à jour sa date de modification s’il existe déjà.

## Arguments

Un ou plusieurs noms de fichiers.

## Options ou éléments utiles

`-c` : ne crée pas le fichier s’il est absent ; `-a` : modifie seulement la date d’accès ; `-m` : seulement la date de modification.

## Exemple du cours

```bash
touch mon_fichier.txt
```

## Vigilances et repères

`touch` ne crée pas les dossiers parents. Créer un fichier existant ne supprime pas son contenu.
