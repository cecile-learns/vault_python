
# `shift()` — Décaler les valeurs

## Fonction

Décale les valeurs d'une `Series` ou d'un `DataFrame` d'un nombre défini de lignes, sans modifier leur index par défaut.

**Usages :**

- Récupérer la valeur précédente ou suivante.
    
- Créer des variables retardées (_lags_) pour les séries temporelles.
    
- Comparer deux observations successives.
    

## Syntaxe

```python
df["colonne"].shift(1)
```

|Paramètre|Rôle|
|---|---|
|`periods=1`|Décalage d'une ligne vers le bas (défaut)|
|`periods=n`|Décalage de n lignes vers le bas|
|`periods=-1`|Décalage d'une ligne vers le haut|
|`fill_value=...`|Valeur utilisée pour remplir les positions laissées vides|

## Exemple — Créer un lag

```python
df["lag1"] = df["passagers"].shift(1)
```

|Mois|passagers|lag1|
|---|---|---|
|Janvier|100|NaN|
|Février|120|100|
|Mars|150|120|
|Avril|130|150|

La valeur du mois précédent devient une variable explicative pour prédire celle du mois courant.

## Exemple — Calculer une variation

```python
df["variation"] = df["passagers"] - df["passagers"].shift(1)
```

Équivalent pour des données numériques à :

```python
df["variation"] = df["passagers"].diff()
```

## Vigilances

- `shift(12)` décale de **12 observations**, pas automatiquement de 12 mois.
    
- Les premières valeurs deviennent `NaN` avec un décalage positif.
    
- Un décalage négatif utilise des observations suivantes : attention aux fuites de données en prédiction.
    

