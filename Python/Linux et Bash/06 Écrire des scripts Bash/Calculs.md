# Calculs

## Pour faire quoi ?

Effectuer une opération arithmétique entière dans un script Bash.

## Arguments

Les nombres et opérateurs sont placés dans l’expression arithmétique.

## Options ou éléments utiles

`$(( expression ))` substitue un résultat ; `let expression` modifie une variable ; `expr` est une commande ancienne qui exige des espaces. Opérateurs usuels : `+`, `-`, `*`, `/`, `%`, `**`.

## Exemple du cours

```bash
a=7
b=3
resultat=$((a + b))
echo "$resultat"
```

## Vigilances et repères

Bash travaille principalement sur des entiers. Pour les décimaux, il faut un outil adapté comme `bc`, Python ou awk.
