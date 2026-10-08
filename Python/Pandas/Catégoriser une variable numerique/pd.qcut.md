# pd.qcut()

## Syntaxe

```python
serie_categorisee = pd.qcut(
    df["col_numerique"],
    q=nombre_categories,
    labels=["categorie_1", "categorie_2", "categorie_3"]
)
```

```python
df["col_categorie"] = pd.qcut(
    df["col_numerique"],
    q=3,
    labels=["Bas", "Moyenne", "Haut"]
)
```

## Paramètres

- `df["col_numerique"]` : série numérique à catégoriser.
- `q` : nombre de groupes basés sur les quantiles. Par exemple, `q=4` découpe les données en quartiles.
- `labels` : noms attribués aux catégories, dans l’ordre croissant des valeurs. Leur nombre doit correspondre au nombre de groupes créés.

## Fonction

Catégorise une variable numérique selon sa distribution en créant des groupes basés sur les quantiles.

## Vigilances & tips

`pd.qcut()` et `pd.cut()` ne répondent pas au même besoin :

- `pd.cut()` → les seuils numériques sont choisis explicitement.
- `pd.qcut()` → les seuils sont déterminés à partir des quantiles de la distribution.

Des valeurs identiques nombreuses peuvent empêcher `pd.qcut()` de créer exactement le nombre de groupes demandé.

**S'utilise souvent avec :** `value_counts()`, `groupby()`
