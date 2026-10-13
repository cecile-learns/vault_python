# load_dataset

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Charger un petit jeu de données d’exemple fourni par le dépôt en ligne de Seaborn.

## Syntaxe réutilisable

```python
df = sns.load_dataset('tips', cache=True)
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `name` | Nom du jeu de données, par exemple `'tips'`, `'penguins'` ou `'titanic'`. |
| `cache` | Réutilise une copie locale si `True`. |
| `data_home` | Dossier de cache personnalisé. |
| `kws` | Autres paramètres transmis à `pandas.read_csv()`. |

## Fonction

Télécharge le fichier CSV d’exemple et renvoie un DataFrame Pandas.

## Vigilances & tips

- Cette fonction nécessite Internet lors du premier chargement.
- Elle sert aux exemples et exercices, pas au chargement de vos propres fichiers.
- Pour vos données, utiliser les fonctions de lecture de Pandas adaptées au format.

## Documentation officielle

[Seaborn — `load_dataset`](https://seaborn.pydata.org/generated/seaborn.load_dataset.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
