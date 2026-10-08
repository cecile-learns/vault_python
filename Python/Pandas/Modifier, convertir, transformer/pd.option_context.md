# `pd.option_context()`

## Syntaxe

```python
with pd.option_context(
    "display.max_rows", 100,
    "display.max_columns", None
):
    print(df)
```

Une seule option :

```python
with pd.option_context("display.max_rows", None):
    print(df)
```

## Paramètres

Les arguments sont fournis par paires :

```python
"nom_option", valeur
```

- `"display.max_rows"` : option qui contrôle le nombre maximal de lignes affichées.
- `100` : fixe temporairement cette limite à 100 lignes.
- `None` : pour `display.max_rows` ou `display.max_columns`, demande d'afficher toutes les lignes ou colonnes.
- `"display.max_columns"` : contrôle le nombre maximal de colonnes affichées.
- Le bloc `with` : délimite la zone dans laquelle les options temporaires s'appliquent.

## Fonction

Modifie temporairement des options pandas uniquement à l'intérieur du bloc `with`. Les réglages précédents sont automatiquement restaurés à la sortie.

## Vigilances & tips

Contrairement à `pd.set_option()`, la modification n'est **pas persistante** pour la suite du notebook.

Pratique pour inspecter ponctuellement un DataFrame complet sans modifier durablement les réglages d'affichage.

**S'utilise souvent avec :** `pd.set_option()`, `pd.get_option()`
