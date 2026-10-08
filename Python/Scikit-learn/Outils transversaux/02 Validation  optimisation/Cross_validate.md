# Validation croisée

## Quand l’utiliser ?

Estimer la variabilité des performances sur le train et comparer des modèles sans consommer le test final.

## Principe

Chaque pli sert à tour de rôle de validation pour un modèle entraîné sur les autres. cross_validate renvoie des scores ; cross_val_predict renvoie des prédictions hors pli.

> Cours : régression du prix des automobiles. L’exemple ci-dessous ne préapprend ni scaler ni sélection avant les plis.

Les blocs du déroulé se lisent et s’exécutent **dans l’ordre**, dans le même notebook. Adapter les chemins des fichiers du cours. Le pattern minimal final est un raccourci distinct, lorsque ses prérequis sont déjà remplis.

---


# Pattern minimal

Avec un estimateur et un train définis :

```python
scores = cross_validate(modele, X_train, y_train, cv=5, scoring="r2")
score_moyen = scores["test_score"].mean()

cross_val_score(lr,X_train,y_train).mean())
```


## Paramètres utiles

- `n_splits` : nombre de plis ; `shuffle` et `random_state` mélangent de façon reproductible.
- `scoring` : métriques ; exemples classification du cours : `"accuracy"`, `"f1"` (binaire), ou `"f1_macro"` pour plusieurs classes.
- `return_train_score=True` : retourne aussi les scores d’apprentissage.
- `test_*` désigne les plis de validation, pas X_test final.
- `cross_val_score` : un score par pli ; `cross_val_predict` : une prédiction hors pli par observation.

## Interprétation

Un score de type neg_* est opposé à une erreur positive car sklearn maximise les scores. Les prédictions hors pli ne proviennent pas d’un seul modèle final.


## Points de vigilance

- Avec une validation croisée, le prétraitement doit être réappris dans chaque pli. Standardiser une fois tout le train avant la CV laisse les plis de validation influencer le prétraitement ; le test final reste séparé, mais le score de CV peut être optimiste. Les pipelines seront traités ultérieurement.
- KFold n’est pas stratifié. Pour un classifieur, cv=5 utilise par défaut des plis stratifiés ; vérifier malgré tout les effectifs et la pertinence du découpage.
- Groupes et chronologie exigent un découpage spécifique.
- Ces trois appels sont des variantes de sortie ; ils réentraînent chacun des modèles et ne sont pas tous nécessaires dans un même travail.


## Références

Source de formation : `01_RESSOURCES/Liora/Modèle de regression.md`.

- [model_selection.KFold](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.KFold.html)
- [model_selection.cross_validate](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_validate.html)
- [model_selection.cross_val_score](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_val_score.html)
- [model_selection.cross_val_predict](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_val_predict.html)




```
scores = cross_validate(    modele, X_train, y_train,    cv=5,    scoring="r2")score_moyen = scores["test_score"].mean()
```

`cross_validate()` renvoie plusieurs informations dans un dictionnaire, notamment :

```
fit_time
score_time
test_score
```


`cross_val_score()` est une version plus simple : il renvoie **directement les scores obtenus à chaque fold**.

Conceptuellement :

```
cross_validate()
    ↓
{"test_score": [R²₁, R²₂, R²₃, R²₄, R²₅], ...}
                  ↓
       ["test_score"].mean()


cross_val_score()
    ↓
[R²₁, R²₂, R²₃, R²₄, R²₅]
                  ↓
               .mean()
```

