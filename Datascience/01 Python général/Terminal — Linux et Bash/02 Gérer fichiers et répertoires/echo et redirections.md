# echo et redirections

## Pour faire quoi ?

Afficher du texte ou envoyer ce texte vers un fichier.

## Arguments

`echo` prend le texte ou la variable à afficher. Après `>` ou `>>`, le chemin du fichier est la destination.

## Options ou éléments utiles

`$variable` affiche une variable ; `>` crée ou remplace le contenu ; `>>` ajoute à la fin ; `-e` interprète des séquences comme `\n` sur de nombreux systèmes.

## Exemple du cours

```bash
echo "Bonjour" > message.txt
echo "Deuxième ligne" >> message.txt
```

## Vigilances et repères

`>` écrase le contenu existant : utiliser `>>` lorsqu’il faut conserver ce qui est déjà écrit.
