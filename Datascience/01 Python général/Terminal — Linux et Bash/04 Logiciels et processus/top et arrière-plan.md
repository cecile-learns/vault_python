# top et arrière-plan

## Pour faire quoi ?

Suivre les processus en direct et laisser une commande tourner en arrière-plan.

## Arguments

`top` ne demande pas d’argument courant. Ajouter `&` à la fin d’une commande l’envoie en arrière-plan.

## Options ou éléments utiles

Dans `top` : `q` quitte ; `k` demande un arrêt ; `M` trie par mémoire ; `P` par processeur. `Ctrl+C` interrompt la commande au premier plan.

## Exemple du cours

```bash
top
commande_longue &
```

## Vigilances et repères

Un processus en arrière-plan reste lié à la session selon le contexte. Contrôler son PID avec `ps` si besoin.
