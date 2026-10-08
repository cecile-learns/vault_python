# `pd.get_dummies()`

## Syntaxe
```python
import pandas as pd
X_train_enc = pd.get_dummies(X_train, columns=['Ville'], dtype=int)
X_test_enc = pd.get_dummies(X_test, columns=['Ville'], dtype=int)
X_test_enc = X_test_enc.reindex(columns=X_train_enc.columns, fill_value=0)
```

## Paramètres
- `columns` : colonnes à encoder.
- `dtype=int` : obtenir des 0/1 plutôt que des booléens.
- `drop_first=True` : supprime une modalité par variable.
- `dummy_na=True` : indicateur de valeur manquante.

## Fonction
Transforme une variable nominale à **k modalités en k indicateurs** (ou k−1 avec `drop_first=True`). N'utilise pas la cible.

## Vigilances & tips
- `reindex` aligne les colonnes du test, mais une catégorie inconnue y devient tout-zéro.
- En validation croisée et production, préférer `OneHotEncoder` dans un `Pipeline`.
- **S'utilise avec :** `reindex()`, `OneHotEncoder`.
