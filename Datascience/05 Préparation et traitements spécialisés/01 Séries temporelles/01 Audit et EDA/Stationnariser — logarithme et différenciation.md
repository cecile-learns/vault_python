# Stationnariser — logarithme et différenciation

## Pourquoi ?

De nombreux modèles classiques supposent que les propriétés de la série restent assez stables dans le temps.

## Stabiliser l'amplitude

```python
serie_log = np.log(serie)
```

La transformation logarithmique exige des valeurs strictement positives.

## Retirer une tendance

```python
serie_diff = serie.diff().dropna()
```

## Différenciation saisonnière

```python
serie_diff_saison = serie.diff(12).dropna()
```

## Vigilances

- conserver la transformation appliquée pour revenir ensuite à l'échelle d'origine ;
- choisir l'ordre de différenciation avec parcimonie ;
- vérifier le résultat graphiquement et avec un test adapté ;
- apprendre toute transformation uniquement sur la partie d'entraînement lors d'une évaluation.
