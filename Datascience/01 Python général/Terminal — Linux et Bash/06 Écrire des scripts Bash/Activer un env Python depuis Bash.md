
# 
### Activer / désactiver un environnement Python depuis 
```Bash

## Activer
source .venv/bin/activate
````

Effet :

- modifie le shell courant ;
- ajoute l’environnement virtuel Python au `PATH` ;
- le terminal affiche généralement `(.venv)`.

## Désactiver

```
deactivate
```

Effet :

- quitte l’environnement virtuel ;
- revient à l’environnement Python système / précédent.

## À retenir

`source` n’est pas spécifique à Python :  
il sert à exécuter un fichier dans le shell courant.

Pattern :

```
source chemin/vers/script
```

