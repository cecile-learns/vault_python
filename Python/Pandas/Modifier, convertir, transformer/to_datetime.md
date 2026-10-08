# to_datetime

## Syntaxe

```python
serie_date = pd.to_datetime(df['col_date'], dayfirst=True, errors='coerce')


df['Join_Date'] = pd.to_datetime(df['Join_Date'])
df['Experience'] = 2024 - df['Join_Date'].dt.year
display(df[['Name', 'Join_Date', 'Experience']])

```

`errors='coerce'` remplace les valeurs non convertibles par `NaN`.
## Fonction

Convertit une Series en dates Pandas.

### Paramètres
`errors` : Détermine quoi faire si une valeur ne peut pas être convertie.
- 'raise' : par défaut, provoque une erreur
- 'coerce' : transforme les valeurs non convertibles en NaT
- 'ignore' : à éviter : ignore les erreurs
 `dayfirst` : Utile pour les dates écrites au format européen.
`yearfirst` : Indique que l’année doit être interprétée en premier lorsque le format est ambigu.
`format` : ex :format="%d/%m/%Y , Permet d’indiquer explicitement le format attendu.
- %d → jour
- %m → mois
- %Y → année sur 4 chiffres
- %H → heure
- %M → minutes
- %S → secondes
`utc` : Convertit les dates dans le fuseau UTC.
`unit` : À utiliser lorsque les dates sont stockées sous forme de timestamp numérique.
- "s"  → secondes
- "ms" → millisecondes
- "us" → microsecondes
- "ns" → nanosecondes
