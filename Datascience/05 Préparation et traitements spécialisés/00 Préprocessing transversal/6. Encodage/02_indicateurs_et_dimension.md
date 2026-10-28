# Encodages indicateurs et réduction de dimension

## Syntaxe

### One-Hot Encoding
```python
from sklearn.preprocessing import OneHotEncoder
enc = OneHotEncoder(handle_unknown='ignore', sparse_output=False)

cats_cols=['variable']

X_train_enc=enc.fit_transform(X_train[cat_cols])
X_test_enc=enc.transform(X_test[cat_cols])

X_train_enc_df= pd.DataFrame (X_train_enc, columns=enc.get_feature_names_out(cat_cols), index=X_train.index)
X_test_enc_df= pd.DataFrame(X_test_enc, columns=enc.get_feature_names_out(cat_cols), index=X_test.index)

X_train = pd.concat([X_train.drop(columns=cat_cols), X_train_enc_df], axis=1)
X_test = pd.concat([X_test.drop(columns=cat_cols), X_test_enc_df], axis=1)
```

### One-Hot avec plusieurs variables catégorielles
```python
from sklearn.compose import ColumnTransformer
preproc = ColumnTransformer([
    ('cat', OneHotEncoder(handle_unknown='ignore'), ['Ville', 'Secteur'])
], remainder='passthrough')
X_train_enc = preproc.fit_transform(X_train)
X_test_enc = preproc.transform(X_test)
```
### One-Hot avec LabelEncoder 
```python
from sklearn.preprocessing import LabelEncoder
from tensorflow.keras.utils import to_categorical

# Encodage : string -> int -> one-hot

label_encoder = LabelEncoder()
y_int = label_encoder.fit_transform(y)          # ex: Iris-setosa -> 0, ...
num_classes = len(np.unique(y_int))             # ici 3
y_cat = to_categorical(y_int, num_classes=num_classes) # représentation one-hot


from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder(sparse_output=False)
y = encoder.fit_transform(df[["Species"]])
```

### Hashing Encoding
```python
from category_encoders import HashingEncoder
enc = HashingEncoder(cols=['Ville'], n_components=8)
X_train_enc = enc.fit_transform(X_train)
X_test_enc = enc.transform(X_test)
```

## Comparaison
| Méthode | Nombre de colonnes | Choix |
|---|---|---|
| One-Hot | k (ou k−1) | Nominales, cardinalité modérée |
| One-Hot plusieurs colonnes | Somme des catégories | Plusieurs variables nominales : même méthode |
| Binary | Environ `ceil(log2(k+1))` | Forte cardinalité, moins de colonnes |
| Hashing | `n_components` fixé | Très forte cardinalité, modalités nouvelles |

## Paramètres
- `handle_unknown='ignore'` : catégorie inconnue représentée par des zéros.
- `min_frequency`, `max_categories` : limiter les modalités rares avec One-Hot.
- `n_components` : dimension du Hashing ; augmenter réduit les collisions.
- `cols` : variables traitées dans `category_encoders`.

## Vigilances & tips
- « One Hot with many categorical » n'est **pas une technique différente**.
- Binary produit des codes arbitraires ; ne pas y voir une hiérarchie.
- Hashing peut créer des **collisions** (deux catégories avec la même représentation).
- Le One-Hot ne conserve pas explicitement la proximité décembre–janvier.
- `fit` uniquement sur train, puis `transform` sur test ; `Pipeline` en validation croisée.
