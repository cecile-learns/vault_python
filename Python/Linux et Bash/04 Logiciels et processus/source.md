

# `source` — exécuter un script dans le shell courant


## À quoi sert `source` ?

`source` permet d’exécuter le contenu d’un fichier **dans le shell courant**.

```
source chemin/vers/fichier
```

Forme équivalente :

```
. chemin/vers/fichier
```

## Pourquoi c’est utile ?

Quand un script doit **modifier l’environnement actuel du terminal**, il faut utiliser `source`.

Exemple fréquent avec un environnement virtuel Python :

```
source .venv/bin/activate
```

Effet :

```
avant
naude@pcecile:~/formation$

        ↓ source .venv/bin/activate

après
(.venv) naude@pcecile:~/formation$
```

Le script `activate` modifie notamment le `PATH` du shell courant pour que les commandes `python` et `pip` utilisent celles de `.venv`.

---

## Différence avec exécuter directement un script

```
./script.sh
```

ou

```
bash script.sh
```

créent normalement un **nouveau processus shell**.

Les modifications faites à l’environnement dans ce sous-shell disparaissent ensuite.

```
shell courant
│
├── lance script.sh
│      └── modifications temporaires
│
└── retour au shell courant
       → modifications perdues
```

Avec :

```
source script.sh
```

le fichier est exécuté **directement dans le shell courant** :

```
shell courant
│
└── source script.sh
       ↓
   modifications conservées
```

## Exemple simple

Fichier `variables.sh` :

```
MESSAGE="Bonjour"
```

Exécution classique :

```
bash variables.sh
echo "$MESSAGE"
```

`MESSAGE` n’est pas disponible dans le shell courant.

Avec `source` :

```
source variables.sh
echo "$MESSAGE"
```

Résultat :

```
Bonjour
```

---

## Cas d’usage fréquents

```
source .venv/bin/activate
```

→ activer un environnement virtuel Python.

```
source ~/.bashrc
```

→ recharger la configuration Bash sans fermer le terminal.

```
source variables.sh
```

→ charger des variables ou fonctions définies dans un autre fichier.

---

## Pattern à retenir

> Pour charger un fichier qui doit modifier **mon terminal actuel** → `source`

```
source fichier
```

À ne pas confondre avec :

```
bash fichier
```

qui exécute le fichier dans un autre processus shell.

## Exemple lié à ton environnement

Activation :

```
source .venv/bin/activate
```

Désactivation :

```
deactivate
```

`deactivate` est disponible après activation de l’environnement virtuel.

