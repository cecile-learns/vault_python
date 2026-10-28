# read_csv

## Syntaxe

```python
df = pd.read_csv('donnees.csv', sep=',')
```

## Paramètres
```python
pd.read_csv(filepath_or_buffer , sep = ',', header = 0, index_col = 0 ... )
```
- `filepath_or_buffer` : Le **chemin d'accès du fichier** .csv relativement à l'environnement d'exécution. Si le fichier se trouve dans le même dossier que l'environnement Python, il suffit de renseigner le nom du fichier (ex : `'mon_dataframe.csv'`). Ce chemin doit être renseigné sous forme de **chaîne de caractères**.
    
- `sep` : Le caractère utilisé dans le fichier .csv pour **séparer** les différentes colonnes. Cet argument doit être spécifié sous forme de **caractère**.
    
- `header` : Le **numéro de la ligne qui contient les noms des colonnes**. Si par exemple les noms de colonnes sont renseignés dans la première ligne du fichier `.csv`, alors il faut spécifier **`header = 0`**. Si les noms ne sont pas renseignés, on mettra `header = None`.
    
- `index_col` : Le **nom ou numéro de la colonne** contenant les **indices** de la base de données. Si les entrées de la base sont indexées par la première colonne, il faudra renseigner **`index_col = 0`**. Alternativement, si les entrées sont indexées par une colonne qui porte le nom `"Id"`, on pourra spécifier **`index_col = "Id"`**.

## Fonction

Importe un fichier CSV dans un DataFrame.
