# cp

## Pour faire quoi ?

Copier un fichier ou un répertoire.

## Arguments

Premier argument : source ; dernier argument : destination.

## Options ou éléments utiles

`-r` : copie un répertoire et son contenu ; `-i` : demande confirmation en cas d’écrasement ; `-v` : affiche les copies.

## Exemple du cours

```bash
cp -i notes.txt sauvegarde/
```

## Vigilances et repères

Si la destination est un dossier existant, le fichier y est copié. Sans `-i`, un fichier de même nom peut être remplacé.
