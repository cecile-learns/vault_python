# Conditions

## Pour faire quoi ?

Exécuter une branche de script selon qu’une condition est vraie ou fausse.

## Arguments

Une condition suit `if` entre crochets, puis les blocs sont délimités par `then`, `else`, `elif` et `fi`.

## Options ou éléments utiles

Tests numériques : `-eq`, `-ne`, `-lt`, `-le`, `-gt`, `-ge`. Tests de chaînes : `=`, `!=`, `-z`, `-n`. Tests de fichiers : `-e`, `-f`, `-d`. `&&` signifie ET, `||` signifie OU.

## Exemple du cours

```bash
if [ "$age" -ge 18 ]; then
  echo "Majeur"
else
  echo "Mineur"
fi
```

## Vigilances et repères

Les espaces dans `[ condition ]` sont obligatoires. Employer `[[ ... ]]` peut être plus confortable en Bash, mais le cours utilise la forme `[ ... ]`.
