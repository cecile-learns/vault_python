# Variables, guillemets et substitutions

## Pour faire quoi ?

Stocker une valeur, la réutiliser et intégrer le résultat d’une commande dans du texte.

## Arguments

Affectation : `nom=valeur`, sans espaces autour de `=`. Lecture : `$nom` ou `${nom}`.

## Options ou éléments utiles

Guillemets doubles : développent `$variable` et `$(commande)` ; guillemets simples : gardent le texte littéral ; `$(commande)` récupère une sortie ; `#` commence un commentaire.

## Exemple du cours

```bash
prenom="Ada"
echo "Bonjour $prenom"
aujourdhui=$(date)
```

## Vigilances et repères

Toujours citer une variable qui peut contenir des espaces : `"$nom"`. La forme `$(...)` est préférable aux anciennes apostrophes inversées.
