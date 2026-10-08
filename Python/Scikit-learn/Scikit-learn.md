# Scikit-learn

Choisir une famille de problème, puis une technique pour retrouver **son déploiement complet**, de la préparation à l’évaluation. Cette branche utilise un format distinct des micro-fiches de syntaxe : code à chaque étape, interprétation des résultats, paramètres utiles, puis véritable pattern minimal final. Les probabilités et le choix du seuil sont séparés des métriques.

- [[Classification|1]]
- [[Scikit-learn/Régression/Régression|Régression]]
- [[Scikit-learn/Clustering/Clustering|Clustering]]
- [[Scikit-learn/Réduction de dimension/Réduction de dimension|Réduction de dimension]]
- [[Outils transversaux|0]]

## Conventions de lecture

- `X` : observations en lignes, variables explicatives en colonnes ; `y` : cible, hors de X.
- Chaque déroulé indique ses données de départ et se lit dans l’ordre ; les fichiers du cours doivent être disponibles au chemin indiqué. Le pattern minimal final suppose uniquement les objets explicitement annoncés.
- `fit` apprend, `transform` transforme, `predict` prédit. `fit_transform` combine apprentissage et transformation.
- Les valeurs de paramètres sont des points de départ ou des exemples du cours, pas des réglages universels.
- Le test final ne sert pas à choisir les hyperparamètres, les variables ou le seuil.
- Les noms terminés par `_` désignent généralement des attributs disponibles après apprentissage.
- Les fiches de technique reprennent les préparations et métriques nécessaires pour être autonomes. Outils transversaux conserve les explications communes et les variantes approfondies.

## Périmètre et sources

- `01_RESSOURCES/Liora/Modèle de classification.md`
- `01_RESSOURCES/Liora/Modèle de regression.md`
- `01_RESSOURCES/Liora/Clustering.md`
- `01_RESSOURCES/Liora/Réduction de dimension.md`

Cette branche reprend les API rencontrées dans ces quatre ressources. Les versions de régression SVR, KNN et forêt y sont citées sans exercice complet : leur fiche le précise. Les jeux de données de démonstration (load_digits, make_swiss_roll) restent du contexte, sans micro-fiche dédiée.

XGBoost, UMAP, scikitplot et les graphiques hiérarchiques SciPy sont identifiés dans les supports mais ne deviennent pas des sous-packages sklearn. LCE est annoncé dans l’introduction sans développement exploitable dans le fichier de classification ; aucune fiche n’est inventée. Les fonctions SciPy nécessaires à un pattern déjà présent (cdist pour le coude) restent explicitement nommées comme telles.

## Écarts utiles avec les supports

- AdaBoost : `estimator` remplace l’ancien `base_estimator`.
- Bagging : `oob_score_` est ici une accuracy, pas une erreur.
- Clustering : le calcul de distorsion du cours est une distance moyenne ; ce n’est pas SSW/(SSW+SSB).
- Les scores de CV impliquant un prétraitement global sont accompagnés d’une réserve : chaque pli doit apprendre son prétraitement. Les pipelines seront ajoutés plus tard.
- Les sources originales ne sont pas corrigées ; ces précisions figurent seulement dans le Vault.

Documentation API vérifiée le 30/09/2026 ; adapter à la version installée si le notebook du cours utilise une version plus ancienne.

## Déroulés et adaptations

Les exemples reprennent les jeux de données des cours. Les adaptations (bornes incluses, noms explicites, contrôle des colonnes, évaluation du modèle après sélection, variantes sur pixels pour la CV) sont signalées dans les fiches. Les prétraitements internes à la validation croisée ne sont pas assimilés à une préparation globale du train ; leurs limites restent explicites.
