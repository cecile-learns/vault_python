
#### `ColumnTransformer`

> Lorsque l'on construit une `pipeline` que l'on entraine sur le jeu de données entier, **toutes** les étapes de la `pipeline`sont effectuées sur **toutes** les variables si rien n'est précisé.
> 
> Cependant, la plupart du temps, on est amené à traiter les données de facons différentes. Par exemple, si l'on a un `DataFrame` avec des variables **qualitatives** et **quantitatives**, on va vouloir encoder les variables **qualitatives** d'un coté, et **standardiser** les données **quantitatives** de l'autre. `ColumnTransformer` nous permet de créer un transformeur qui va appliquer une transformation que sur certaines colonnes indiquées.
> 
> L'initisation d'un objet `ColumnTransformer` se fait en passant une liste de tuples à 3 élements en argument de son constructeur : `ColumnTransformer(transformers = [('transformation', transformeur, ['colonne 1', 'colonne 2',...]))`
> 
> ```python
>     from sklearn.compose import ColumnTransformer
>     from sklearn.compose import make_column_transformer
>     # Standardisation des valeurs
>     numerical_st = StandardScaler()  
>     # Encodage des variables catégorielles 
>     categorical_ohe = OneHotEncoder()
>     prepro = ColumnTransformer(
>         #num_vars : liste des variables numériques
>         #cat_vars : liste des variables catégorielles
>         transformers = [('numerical', numerical_st, num_vars), 
>                        ('categorical', categorical_ohe, cat_vars)])                        
> ```

#### `make_column_transformer`

> La fonction `make_column_transformer()` permet de faire la même chose. C'est un raccourci du constructeur `ColumnTransformer`. La principale différence entre les deux est qu'avec `make_column_transformer()`, on ne peut pas **nommer les transformeurs** ce qui peut être utile si l'on veut entrainer une **grille de recherche**.
> 
> En général, on utilisera `make_column_transformer` si l'on a pas besoin d'utiliser une **grille de recherche**, sinon on priviliégera `ColumnTransformer`.
> 
> La fonction `make_column_transformer` a presque la même syntaxe que `ColumnTransformer` sauf qu'on ne précise pas le nom des transformeurs : `make_column_transformer((transformer, ['colonne 1', 'colonne 2,...]))`
> 
> ```python
>     from sklearn.compose import make_column_transformer
>     # Standardisation des valeurs
>     numerical_st = StandardScaler()  
>     # Encodage des variables catégorielles 
>     categorical_ohe = OneHotEncoder()
>     prepro = make_column_transformer((numerical_st, num_vars), #num_var : liste des variables numériques
>                                 (categorical_ohe, cat_var)) #cat_var : liste des variables catégorielles
> ```