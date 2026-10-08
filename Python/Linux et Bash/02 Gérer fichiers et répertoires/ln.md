

# ln — créer des liens

`ln` permet de créer un autre accès vers un fichier ou un dossier.

## Lien symbolique

```bash
ln -s cible lien
````

Exemple :

```
ln -s ../ressources/data.csv data.csv
```

Crée dans le dossier courant un lien `data.csv` qui pointe vers :

```
../ressources/data.csv
```

On peut aussi écrire :

```
ln -s ../ressources/data.csv .
```

Le `.` signifie : créer le lien dans le dossier courant.

## Modèle mental

```
prod/data.csv
     │
     └──→ ressources/data.csv
```

Le fichier n'est pas copié.

Si le fichier cible est modifié, le changement est visible via le lien.

Si le fichier cible est supprimé ou déplacé, le lien devient cassé.

## Vérifier un lien

```
ls -l
```

Exemple :

```
data.csv -> ../ressources/data.csv
```

## Lien physique

```
ln fichier nouveau_nom
```

Les deux noms désignent alors le même fichier sur le disque.

À retenir surtout :

```
ln -s
```

= créer un **lien symbolique**, comparable à un raccourci.

```

Pour le Vault, je la classerais simplement sous quelque chose comme `[[Linux & Bash]] > Fichiers et répertoires > ln`.
```