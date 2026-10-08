# Tableaux

## Pour faire quoi ?

Conserver plusieurs valeurs dans une variable Bash indexée.

## Arguments

Affectation : `tableau=(valeur1 valeur2)`. Un élément s’obtient par `${tableau[index]}`.

## Options ou éléments utiles

`${tableau[@]}` donne tous les éléments ; `${!tableau[*]}` donne les indices ; `${#tableau[*]}` donne le nombre d’éléments ; `tableau+=(valeur)` ajoute une valeur.

## Exemple du cours

```bash
couleurs=(rouge vert bleu)
echo "${couleurs[1]}"
echo "${couleurs[@]}"
```

## Vigilances et repères

Les indices commencent à `0`. Mettre les expansions entre guillemets, notamment `"${tableau[@]}"`, pour préserver les valeurs contenant des espaces.
