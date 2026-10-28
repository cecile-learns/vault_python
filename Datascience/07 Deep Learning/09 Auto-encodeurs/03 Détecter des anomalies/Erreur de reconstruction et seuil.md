# Erreur de reconstruction et seuil

## Syntaxe

```python
import numpy as np

X_reconstruit = autoencoder.predict(X_test, verbose=0)
scores_anomalie = np.mean((X_test - X_reconstruit) ** 2, axis=1)

reconstruction_train = autoencoder.predict(X_train_normal, verbose=0)
erreurs_train = np.mean(
    (X_train_normal - reconstruction_train) ** 2,
    axis=1,
)

seuil = np.percentile(erreurs_train, 95)
y_pred_anomalie = (scores_anomalie > seuil).astype(int)
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `axis=1` | calcule un score par observation | adapter si les données ne sont pas aplaties |
| percentile `95` | proportion des normales située sous le seuil | augmenter pour moins d'alertes, diminuer pour plus de sensibilité |
| erreur quadratique | pénalise fortement les écarts importants | comparer à une erreur absolue selon le problème |

## Vigilances & tips

Le seuil doit être calculé sur des données d'entraînement normales, pas sur le jeu de test mélangé.
