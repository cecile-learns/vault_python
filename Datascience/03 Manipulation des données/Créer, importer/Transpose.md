# Transposer un DataFrame — `transpose()` / `.T`

## Syntaxe

```python
df_transpose = df.transpose()
```

Forme courte équivalente :

```python
df_transpose = df.T
```

Lorsque la transposition place dans la première ligne les futurs noms de colonnes :

```python
df_transpose = df.transpose()
df_transpose.columns = df_transpose.iloc[0]
df_transpose = df_transpose.iloc[1:]
```

Lorsque des informations utiles se retrouvent dans l'index après transposition :

```python
df_transpose = df.transpose()
df_transpose = df_transpose.reset_index()
df_transpose.columns = ["colonne_1", "colonne_2"]
```

## Paramètres

`df` : DataFrame à transposer.

`transpose()` : méthode Pandas qui échange les lignes et les colonnes du DataFrame.

`.T` : propriété donnant la transposée du DataFrame ; écriture courte utile dans ce cas.

`df_transpose.iloc[0]` : première ligne par position. Elle peut devenir `df_transpose.columns` si elle contient réellement les futurs noms de colonnes.

`df_transpose.iloc[1:]` : conserve les lignes à partir de la deuxième et retire donc la ligne utilisée comme en-tête.

`reset_index()` : remet l'index dans une colonne ordinaire et recrée un index numérique. À utiliser lorsque la transposition a placé une information utile dans l'index.

## À retenir

La transposition échange les axes : `lignes ↔ colonnes`.

Après une transposition destinée à restructurer des données, inspecter le résultat avant de continuer :

```python
print(df_transpose.shape)
print(df_transpose.index)
print(df_transpose.columns)
display(df_transpose.head())
```

Ne pas appliquer automatiquement `reset_index()` ou `df_transpose.columns = df_transpose.iloc[0]`. Le choix dépend de la structure réellement obtenue.

## Exemple du cours — `state.csv`

Dans l'exercice Liora, le fichier `state.csv` doit être restructuré avant la jointure avec le DataFrame principal.

Correction du cours, reprise telle qu'utilisée dans l'exercice :

```python
state = pd.read_csv('state.csv')
state = state.transpose()
state.columns = state.iloc[0]
state = state[1:]
df = df.merge(state, on=['City'], how='left')
```

Ici, après la transposition, la première ligne contient les futurs noms de colonnes. Elle devient donc l'en-tête avec `state.columns = state.iloc[0]`, puis elle est retirée avec `state = state[1:]`.
