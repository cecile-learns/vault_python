# Choisir une méthode de Feature Selection

| Besoin | Famille |
|---|---|
| Filtrer indépendamment du modèle | Méthode Filter : variance, score statistique, `SelectKBest` |
| Tester récursivement des sous-ensembles | Méthode Wrapper : `RFE`, `RFECV` |
| Sélection intégrée à un modèle | Méthode Embedded : `SelectFromModel`, Lasso, importance d'arbres |

Le choix doit être appris uniquement sur les données d'entraînement. La sélection fait partie du préprocessing et doit être répétée dans chaque pli de validation croisée.
