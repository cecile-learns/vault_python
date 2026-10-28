# Fonctions et arguments

## Pour faire quoi ?

Regrouper un traitement réutilisable et recevoir des valeurs lors de l’appel d’un script ou d’une fonction.

## Arguments

Une fonction est définie par `nom() { ...; }`. Dans un script, `$1`, `$2` désignent les premiers arguments ; dans une fonction aussi, ils sont locaux à l’appel.

## Options ou éléments utiles

`local variable=...` limite une variable à la fonction ; `return nombre` renvoie un statut de 0 à 255 ; `echo` permet de produire une valeur récupérable ; `$#` donne le nombre d’arguments.

## Exemple du cours

```bash
saluer() {
  local nom="$1"
  echo "Bonjour $nom"
}

saluer "Ada"
```

## Vigilances et repères

`return` ne sert pas à renvoyer une chaîne ou un grand nombre : utiliser `echo` puis une substitution `$(fonction)` pour cela. Toujours citer `"$1"` si l’argument peut contenir des espaces.
