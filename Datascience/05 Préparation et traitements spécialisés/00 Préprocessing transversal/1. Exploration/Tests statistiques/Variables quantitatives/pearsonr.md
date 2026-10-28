# pearsonr

## Syntaxe

```python
from scipy.stats import pearsonr

donnees_paires = df[['col_mesure_1', 'col_mesure_2']].dropna()

resultat_correlation = pearsonr(
    x=donnees_paires['col_mesure_1'],
    y=donnees_paires['col_mesure_2']
)

coefficient_r = resultat_correlation.statistic
p_value = resultat_correlation.pvalue
```

## Paramètres

- `x` et `y` : deux séries quantitatives appariées, de même longueur.
- `dropna()` est appliqué conjointement aux deux colonnes afin de conserver les mêmes lignes dans `x` et `y`.

## Fonction

Mesure la force et le sens d’une liaison linéaire. `statistic` est le coefficient de Pearson, compris entre -1 et 1 ; `pvalue` teste l’hypothèse d’une corrélation linéaire nulle.

## Vigilances & tips

Une corrélation significative n’implique ni causalité ni effet important.

**S'utilise souvent avec :** `spearmanr()`.
