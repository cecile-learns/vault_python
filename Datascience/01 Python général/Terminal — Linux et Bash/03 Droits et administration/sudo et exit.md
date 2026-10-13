# sudo et exit

## Pour faire quoi ?

Exécuter une commande avec des droits administrateur, puis quitter une session administrateur.

## Arguments

Après `sudo`, indiquer la commande à exécuter. `exit` ne prend pas d’argument dans l’usage courant.

## Options ou éléments utiles

`sudo -i` ouvre un shell administrateur ; `sudo su` change vers l’utilisateur administrateur ; `sudo -k` oublie l’autorisation mise en cache.

## Exemple du cours

```bash
sudo apt update
exit
```

## Vigilances et repères

Préférer `sudo commande` à une session administrateur durable. Relire la commande : ses effets peuvent concerner tout le système.
