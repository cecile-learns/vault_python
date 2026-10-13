# ttest_ind

## Syntaxe

```python
from scipy.stats import ttest_ind

resultat_test = ttest_ind(
    a=mesures_groupe_1,
    b=mesures_groupe_2,
    equal_var=False,
    alternative='two-sided',
    nan_policy='omit'
)

statistique_t = resultat_test.statistic
p_value = resultat_test.pvalue
```

## Paramètres

- `a` et `b` : mesures quantitatives provenant de deux groupes indépendants.
- `equal_var=False` : applique le test t de Welch, sans supposer les variances égales.
- `alternative='two-sided'` : teste une différence dans les deux sens ; `'greater'` et `'less'` définissent un test unilatéral.
- `nan_policy='omit'` : ignore les valeurs manquantes.

## Fonction

Compare les moyennes de deux groupes indépendants. `statistic` est la statistique t ; `pvalue` est la p-value associée.

## Vigilances & tips

Ne convient pas aux mesures appariées ou répétées sur les mêmes individus.
