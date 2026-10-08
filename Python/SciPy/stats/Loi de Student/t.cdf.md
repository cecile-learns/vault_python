# t.cdf

## Syntaxe

```python
from scipy.stats import t

probabilite_cumulee = t.cdf(
    x=statistique_t_observee,
    df=nombre_observations - 1
)

p_value_unilaterale_droite = 1 - probabilite_cumulee
```

## Paramètres

- `x=statistique_t_observee` : valeur jusqu’à laquelle la probabilité est cumulée.
- `df=nombre_observations - 1` : degrés de liberté de la loi de Student utilisée ici.

## Fonction

Retourne la probabilité d’obtenir une valeur inférieure ou égale à `x` sous une loi de Student. Le complément à 1 donne ici la probabilité située dans la queue droite.

## Vigilances & tips

La transformation de la probabilité cumulée en p-value dépend du sens du test.
