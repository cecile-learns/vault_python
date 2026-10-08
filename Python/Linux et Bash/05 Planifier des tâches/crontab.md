# crontab

## Pour faire quoi ?

Créer et administrer les tâches planifiées par cron.

## Arguments

`crontab -e` ouvre le planning de l’utilisateur. Une ligne comporte cinq champs de date, puis la commande à exécuter.

## Options ou éléments utiles

`-l` liste les tâches ; 
`-e` édite ;
`-r` supprime le planning ;
`-i` confirme avant suppression ; 
`-u utilisateur` vise un autre utilisateur avec droits adaptés. Champs : minute, heure, jour du mois, mois, jour de semaine.
`*` tous,
`,` liste, 
`-` plage, 
`/` pas.Q

## Exemple du cours

```bash
0 9 * * 1-5 /chemin/vers/script.sh
```

## Vigilances et repères

Utiliser des chemins absolus dans une tâche cron. Tester la commande manuellement avant de la planifier et rediriger la sortie vers un journal si nécessaire.

![[Pasted image 20260924133922.png]]