# Prophet — validation croisée temporelle

## Pattern

```python
from prophet.diagnostics import cross_validation, performance_metrics

resultats_cv = cross_validation(
    modele,
    initial="2920 days",
    period="365 days",
    horizon="365 days",
    parallel="threads",
)

metriques = performance_metrics(resultats_cv, rolling_window=1)
```

## Paramètres

- `initial` : première fenêtre d'apprentissage ;
- `period` : intervalle entre deux dates de coupure ;
- `horizon` : distance de prévision évaluée.

## Vigilances & tips

Les fenêtres respectent l'ordre temporel. Adapter les durées à la fréquence et à la profondeur de l'historique.

Documentation : [diagnostics Prophet](https://facebook.github.io/prophet/docs/diagnostics.html)
