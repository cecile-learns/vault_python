# Shebang et exécution

## Pour faire quoi ?

Indiquer quel interpréteur doit lire le script, puis le lancer.

## Arguments

Le shebang est la première ligne. Pour lancer : chemin du script ou argument après `bash`.

## Options ou éléments utiles

`#!/bin/bash` choisit Bash ; `bash script.sh` ne demande pas le droit d’exécution ; `./script.sh` l’exige généralement, avec `chmod +x script.sh`.

## Exemple du cours

```bash
#!/bin/bash
echo "Bonjour"

bash script.sh
```

## Vigilances et repères

Le shebang doit être la toute première ligne. `./script.sh` ne fonctionne que depuis le dossier courant si le script est exécutable.
