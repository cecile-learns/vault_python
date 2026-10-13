# ps et kill

## Pour faire quoi ?

Observer les processus, puis demander l’arrêt d’un processus précis.

## Arguments

`ps` accepte des options ; `kill` prend le PID, identifiant numérique du processus.

## Options ou éléments utiles

`ps -f` : vue détaillée du terminal ; `ps aux` : processus de tous les utilisateurs ; `kill PID` : arrêt normal ; `kill -9 PID` : arrêt forcé.

## Exemple du cours

```bash
ps aux
kill 1234
```

## Vigilances et repères

Commencer par `kill PID`. `kill -9` ne laisse pas au programme le temps de fermer proprement ou d’enregistrer son état.
