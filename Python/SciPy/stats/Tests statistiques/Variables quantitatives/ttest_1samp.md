# ttest_1samp

## Syntaxe

```python
from scipy.stats import ttest_1samp

resultat_test = ttest_1samp(
    a=serie_mesures,
    popmean=moyenne_theorique,
    alternative='two-sided',
    nan_policy='omit'
)

statistique_t = resultat_test.statistic
p_value = resultat_test.pvalue
```

## Paramètres

- `a=serie_mesures` : échantillon de valeurs quantitatives observées.
- `popmean=moyenne_theorique` : moyenne supposée sous l’hypothèse nulle.
- `alternative='two-sided'` : teste une différence dans les deux sens. Utiliser `'greater'` ou `'less'` pour un test unilatéral.
- `nan_policy='omit'` : ignore les valeurs manquantes lors du calcul.

## Fonction

Teste si la moyenne d’un échantillon diffère d’une valeur théorique. `statistic` est la statistique t ; `pvalue` est la p-value associée au test.

## Vigilances & tips

Le sens de `alternative` doit être fixé à partir de l’hypothèse formulée avant d’observer le résultat.
