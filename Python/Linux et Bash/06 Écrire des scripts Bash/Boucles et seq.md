# Boucles et seq

## Pour faire quoi ?

Répéter une action tant qu’une condition est vraie ou pour chaque valeur d’une série.

## Arguments

`while` reçoit une condition ; `for` reçoit une variable puis une liste ; `seq` reçoit une borne, ou début fin, ou début pas fin.

## Options ou éléments utiles

`while condition; do ... done` ; `for x in liste; do ... done` ; `seq 5`, `seq 2 5`, `seq 0 2 10`.

## Exemple du cours

```bash
for i in $(seq 1 5); do
  echo "$i"
done
```

## Vigilances et repères

Éviter une boucle `while` sans évolution de condition : elle serait infinie. Pour des listes de noms avec espaces, préférer des tableaux à `$(seq ...)` ou à une expansion non citée.
