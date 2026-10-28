# t.ppf

## Syntaxe

```python
from scipy.stats import t

quantile_critique = t.ppf(
    q=1 - (1 - niveau_confiance) / 2,
    df=nombre_observations - 1
)
```

## Paramètres

- `q` : probabilité cumulée associée au quantile recherché ; la formule présentée correspond à un intervalle bilatéral.
- `niveau_confiance` : niveau de confiance souhaité, par exemple `0.95`.
- `df=nombre_observations - 1` : degrés de liberté de la loi de Student.

## Fonction

Retourne le quantile de la loi de Student correspondant à une probabilité cumulée. Il sert ici à obtenir la valeur critique d’un intervalle de confiance bilatéral.

## Vigilances & tips

`ppf()` est la fonction inverse de `cdf()`.
