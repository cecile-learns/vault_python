# Détecter des anomalies par reconstruction

## Pour faire quoi ?

Entraîner un auto-encodeur sur des données normales puis utiliser une mauvaise reconstruction comme signal d'anomalie.

## Pipeline mental

```text
données normales
→ entraînement de l'auto-encodeur
→ reconstruction
→ erreur par observation
→ seuil calculé sur les normales
→ prédiction normale / anomalie
```

## Script réutilisable

```python
import numpy as np
from sklearn.metrics import classification_report, confusion_matrix

autoencoder.fit(
    X_train_normal,
    X_train_normal,
    epochs=30,
    batch_size=128,
    validation_split=0.2,
)

train_reconstruit = autoencoder.predict(X_train_normal, verbose=0)
erreurs_train = np.mean((X_train_normal - train_reconstruit) ** 2, axis=1)
seuil = np.percentile(erreurs_train, 95)

test_reconstruit = autoencoder.predict(X_test, verbose=0)
scores_test = np.mean((X_test - test_reconstruit) ** 2, axis=1)
y_pred = (scores_test > seuil).astype(int)

print(confusion_matrix(y_test_anomalie, y_pred))
print(classification_report(y_test_anomalie, y_pred))
```

## Paramètres et hyperparamètres

| Paramètre | Effet |
|---|---|
| percentile du seuil | compromis faux positifs / anomalies manquées |
| fonction d'erreur | définition opérationnelle de l'écart |
| capacité de l'auto-encodeur | trop forte : il peut aussi reconstruire les anomalies |

## Vigilances & tips

Le modèle doit apprendre la structure normale. Un train fortement contaminé affaiblit le signal d'anomalie.
