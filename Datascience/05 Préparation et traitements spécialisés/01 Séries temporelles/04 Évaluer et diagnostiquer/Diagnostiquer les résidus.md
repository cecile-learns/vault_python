# Diagnostiquer les résidus

## Principe

```python
residus = y_test - y_pred
```

Un modèle satisfaisant ne doit plus laisser de structure temporelle évidente dans les résidus.

## Contrôles

- résidus centrés autour de zéro ;
- variance suffisamment stable ;
- absence de tendance ou de saisonnalité visible ;
- faible autocorrélation résiduelle ;
- grandes erreurs examinées dans leur contexte métier.

## Interprétation

Une métrique moyenne correcte peut masquer des périodes systématiquement mal prévues. Toujours compléter le score par un graphique des résidus et des erreurs au fil du temps.
