# Déplier un niveau d'index en colonnes — `unstack()`

## Syntaxe

Après un `groupby()` sur plusieurs colonnes :

```python
resultat = (
    df.groupby(["col_groupe_1", "col_groupe_2"])["col_valeur"]
      .sum()
      .unstack()
)
```

Avec remplacement des combinaisons absentes par `0` :

```python
resultat = (
    df.groupby(["col_groupe_1", "col_groupe_2"])["col_valeur"]
      .sum()
      .unstack(fill_value=0)
)
```

## Paramètres

`["col_groupe_1", "col_groupe_2"]` : colonnes servant à former les groupes. Après le `groupby()`, elles constituent plusieurs niveaux d'index.

`["col_valeur"]` : colonne sur laquelle l'agrégation est effectuée.

`.sum()` : agrégation utilisée ici ; elle doit être adaptée à la question posée.

`unstack()` : déplace par défaut le dernier niveau de l'index vers les colonnes.

`fill_value=0` : remplace par `0` les combinaisons de groupes absentes au lieu de laisser une valeur manquante.

## À retenir

Pattern :

```text
groupby sur plusieurs dimensions
→ agrégation
→ MultiIndex
→ unstack()
→ un niveau d'index devient des colonnes
```

Exemple de transformation :

```text
State        marketing_channel
California   Facebook              120
             TV / Radio             80
Florida      Facebook               50
             TV / Radio             30
```

devient :

```text
marketing_channel   Facebook   TV / Radio
State
California               120           80
Florida                   50           30
```

Pour contrôler les niveaux d'index avant la transformation :

```python
print(resultat.index.names)
```

Si l'objectif est seulement de remettre les clés d'un `groupby()` en colonnes ordinaires, utiliser plutôt `reset_index()`. `unstack()` sert à déplacer un niveau d'index vers l'axe des colonnes.
