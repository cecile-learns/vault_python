# spearmanr

## Syntaxe

```python
from scipy.stats import spearmanr

donnees_paires = df[['col_mesure_1', 'col_mesure_2']].dropna()

resultat_correlation = spearmanr(
    a=donnees_paires['col_mesure_1'],
    b=donnees_paires['col_mesure_2']
)

coefficient_rho = resultat_correlation.statistic
p_value = resultat_correlation.pvalue
```

## Paramètres

- `a` et `b` : deux séries quantitatives appariées, transformées en rangs pour le calcul.
- `dropna()` est appliqué conjointement aux deux colonnes afin de conserver les mêmes lignes.

## Fonction

Mesure la force et le sens d’une liaison monotone, pas nécessairement linéaire. `statistic` est le coefficient rho de Spearman ; `pvalue` teste l’hypothèse d’une corrélation de rang nulle.

## Vigilances & tips

**S'utilise souvent avec :** `pearsonr()`.
