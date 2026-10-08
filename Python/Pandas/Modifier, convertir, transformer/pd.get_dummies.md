# `pd.get_dummies()`

## Syntaxe

```python
df_encode = pd.get_dummies(
    df,
    columns=["col_categorie"],
    dtype=int
)
```

Avec une modalité de référence supprimée :

```python
df_encode = pd.get_dummies(
    df,
    columns=["col_categorie"],
    drop_first=True,
    dtype=int
)
```

## Paramètres

- `df` : `DataFrame` contenant la ou les variables catégorielles à encoder.
- `columns=["col_categorie"]` : liste des colonnes à transformer en variables indicatrices. Chaque modalité génère une nouvelle colonne.
- `drop_first=True` : supprime la première modalité de chaque variable encodée. Utile notamment pour éviter des colonnes redondantes dans certains modèles linéaires ; ce n'est pas nécessaire pour tous les modèles.
- `dtype=int` : crée les colonnes indicatrices sous forme d'entiers `0` et `1`.

## Exemple

Pour une colonne :

```text
Ville
Bordeaux
Paris
Bordeaux
```

```python
df_encode = pd.get_dummies(
    df,
    columns=["Ville"],
    dtype=int
)
```

donne notamment :

```text
Ville_Bordeaux  Ville_Paris
1               0
0               1
1               0
```

## Fonction

`pd.get_dummies()` transforme des variables catégorielles en **colonnes indicatrices** utilisables notamment par les modèles de Machine Learning. Il retourne un nouveau `DataFrame`.

## Vigilances & tips

Une variable à plusieurs modalités produit plusieurs nouvelles colonnes : surveiller l'augmentation du nombre de variables.

`drop_first=True` retire une catégorie de référence. Ne pas l'utiliser automatiquement : son intérêt dépend notamment du modèle utilisé.

Pour un pipeline de Machine Learning appliqué ensuite à de nouvelles données, préférer généralement un encodeur intégré au pipeline, comme `OneHotEncoder`.

**S'utilise souvent avec :** `select_dtypes()`, `OneHotEncoder`
