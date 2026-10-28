# f_oneway

## Syntaxe

```python
from scipy.stats import f_oneway

resultat_anova = f_oneway(
    mesures_groupe_1,
    mesures_groupe_2,
    mesures_groupe_3
)

statistique_f = resultat_anova.statistic
p_value = resultat_anova.pvalue
```

## Paramètres

- `mesures_groupe_1`, `mesures_groupe_2`, `mesures_groupe_3` : échantillons quantitatifs des groupes indépendants à comparer. Au moins deux groupes sont nécessaires.

## Fonction

Réalise une ANOVA à un facteur et teste si toutes les moyennes de population sont égales. `statistic` est la statistique F ; `pvalue` indique si les données permettent de rejeter l’égalité globale des moyennes.

## Vigilances & tips

Une p-value significative indique qu’au moins une moyenne diffère, sans identifier laquelle.

Dans le cours, l’ANOVA est réalisée avec `statsmodels.formula.api.ols()` puis `statsmodels.api.stats.anova_lm()`. Cette approche produit une table d’ANOVA détaillée, tandis que `f_oneway()` retourne directement la statistique F et la p-value du test global.
