# pd.cut()

## Syntaxe

```python
serie_categorisee = pd.cut(
    df["col_numerique"],
    bins=[borne_1, borne_2, borne_3, float("inf")],
    labels=["categorie_1", "categorie_2", "categorie_3"]
)
```

```python
df["col_categorie"] = pd.cut(
    df["col_numerique"],
    bins=[borne_1, borne_2, borne_3, np.max(df.variable)],
    labels=["categorie_1", "categorie_2", "categorie_3"]
)
```

## Paramètres

- `df["col_numerique"]` : série numérique à catégoriser.
- `bins` : bornes utilisées pour créer les intervalles. Avec 4 bornes, 3 catégories sont créées.
- `labels` : noms attribués aux intervalles, dans le même ordre que les intervalles définis par `bins`.
- `float("inf")` : représente l’infini positif ; permet de créer une dernière catégorie sans avoir à connaître la valeur maximale.
- include_lowest=True permet d'inclure la borne inférieure au premier intervalle

## Exemple

```python
df["MedHouseValCategory"] = pd.cut(
    df["MedHouseVal"],
    bins=[0, 2, 4, float("inf")],
    labels=["Bas", "Moyenne", "Haut"]
)
```

## Fonction

Catégorise une variable numérique à partir de seuils définis explicitement. Retourne une Series catégorielle.

## Vigilances & tips

Le nombre de `labels` doit correspondre au nombre d’intervalles : 4 bornes définissent 3 intervalles.

Par défaut, les intervalles sont fermés à droite : par exemple `(0, 2]` contient `2` mais pas `0`.

Avec `pd.cut()`, les seuils sont imposés par `bins` ; les catégories ne contiennent donc pas nécessairement le même nombre d’observations.

**S'utilise souvent avec :** `value_counts()`, `groupby()`
