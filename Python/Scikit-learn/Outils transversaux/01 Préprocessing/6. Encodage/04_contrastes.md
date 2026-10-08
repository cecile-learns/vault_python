# Encodages par contrastes statistiques

## Syntaxe
```python
from category_encoders import SumEncoder, HelmertEncoder, BackwardDifferenceEncoder

# Choisir UN des trois encodeurs :
enc = SumEncoder(cols=['Groupe'])
# enc = HelmertEncoder(cols=['Groupe'])
# enc = BackwardDifferenceEncoder(cols=['Groupe'])

X_train_enc = enc.fit_transform(X_train)
X_test_enc = enc.transform(X_test)
```

## Comparaison
| Méthode | Colonnes | Principe |
|---|---|---|
| Sum | k−1 | Effets autour de la moyenne non pondérée des modalités |
| Helmert | k−1 | Comparaison successive d'une modalité aux précédentes |
| Backward Difference | k−1 | Comparaison de modalités consécutives |

## Paramètres
- `cols` : variables catégorielles concernées.
- L'ordre des catégories conditionne les contrastes Helmert et Backward Difference.
- `handle_unknown`, `handle_missing` : gestion des catégories inconnues/manquantes (vérifier les options de la version installée).

## Fonction
Encoder une variable catégorielle en **contrastes** pour l'interprétation de modèles statistiques ou linéaires, sans utiliser la cible pour calculer les codes.

## Vigilances & tips
- Ces encodages ne sont **pas réservés aux variables ordinales**.
- Les coefficients ne sont pas systématiquement des différences brutes de moyennes : ils dépendent de la convention et de l'échelle.
- Contrôler l'ordre des catégories lorsque l'interprétation des contrastes importe.
- Pour une prédiction classique sans objectif de contraste, commencer par One-Hot.
- `fit` sur train, `transform` sur test.
