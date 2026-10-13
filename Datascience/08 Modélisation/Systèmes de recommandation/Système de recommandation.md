

# Systèmes de recommandation — Implémentation Python



> [!info] Périmètre
> Filtrage basé sur le contenu, filtrage collaboratif par approche mémoire et factorisation matricielle.
>
> **Statut d'apprentissage :** chapitres 1 et 2 parcourus en lecture exploratoire ; chapitre 3 survolé. Code non maîtrisé et à retester.

---

## 1. Filtrage basé sur le contenu — TF-IDF

### Charger et préparer les descriptions

```python
import pandas as pd
from sklearn.feature_extraction.text import TfidfVectorizer

df = pd.read_csv("goodreads_descr.csv")
df = df.reset_index(drop=True)

df["description"] = df["description"].fillna("")
```

**Entrée :** DataFrame contenant `title` et `description`.

**Sortie :** textes exploitables, une ligne par livre.

### Transformer les descriptions en vecteurs

```python
tfidf = TfidfVectorizer(stop_words="english")
matrice_tfidf = tfidf.fit_transform(df["description"])

print(matrice_tfidf.shape)
```

**Sortie :** matrice creuse `livres × termes`.

`fit` apprend le vocabulaire ; `transform` représente les textes numériquement.

### Calculer une similarité sans saturer la RAM

```python
from sklearn.metrics.pairwise import cosine_similarity

idx = 0  # Position du livre de référence

scores = cosine_similarity(
    matrice_tfidf[idx],
    matrice_tfidf
).ravel()
```

**Sortie :** tableau de N scores, et non matrice N × N.

### Classer les livres

```python
scores[idx] = -1  # Exclure le livre cible

top_idx = scores.argsort()[::-1][:10]

resultats = df.iloc[top_idx][["title"]].copy()
resultats["similarite"] = scores[top_idx]

print(resultats)
```

**Sortie :** DataFrame contenant les dix livres les plus similaires.

### Variante : distance euclidienne

```python
from sklearn.metrics.pairwise import euclidean_distances

distances = euclidean_distances(
    matrice_tfidf[idx],
    matrice_tfidf
).ravel()

scores_euclidiens = 1 / (1 + distances)
```

Distance faible → score transformé élevé.

> [!warning] Mémoire et indices
> Calculer les similarités uniquement pour le livre demandé évite la création d'une matrice dense N × N.
>
> Si le DataFrame est échantillonné, construire la matrice TF-IDF à partir du même sous-ensemble, dans le même ordre.

---

## 2. Filtrage collaboratif — Matrice de notations

### Charger les interactions

```python
df_notes = pd.read_csv("ratings_flrd.csv")

df_notes = df_notes[
    ["user_id", "title", "rating"]
]
```

**Entrée :** une ligne par interaction utilisateur-livre.

### Construire la matrice

```python
mat_ratings = df_notes.pivot_table(
    index="user_id",
    columns="title",
    values="rating",
    aggfunc="mean"
)
```

**Sortie :** DataFrame `utilisateurs × livres`.

Les `NaN` correspondent aux notes non observées.

### Préparer les absences d'interaction

Dans l'exemple du cours, les notes vont de 0 à 5.

```python
mat_ratings = mat_ratings + 1
mat_ratings = mat_ratings.fillna(0)
```

Après décalage :

- `0` : interaction absente ;
- `1` à `6` : notes observées, initialement de 0 à 5.

Ce décalage est une convention du notebook, pas une règle générale pour tous les systèmes de recommandation.

### Convertir au format CSR

```python
from scipy.sparse import csr_matrix

sparse_ratings = csr_matrix(mat_ratings.values)

user_ids = mat_ratings.index
titles = mat_ratings.columns
```

**Sortie :** matrice creuse permettant d'économiser la mémoire lorsque les zéros sont nombreux.

---

## 3. Filtrage collaboratif — User-based

**Question :** quels utilisateurs ont des profils de notation similaires ?

### Calculer les similarités avec un utilisateur

```python
from sklearn.metrics.pairwise import cosine_similarity

user_id = user_ids[0]
position = mat_ratings.index.get_loc(user_id)

scores = cosine_similarity(
    sparse_ratings[position],
    sparse_ratings
).ravel()
```

**Sortie :** un score par utilisateur.

### Retrouver les utilisateurs les plus proches

```python
similar_users = pd.Series(
    scores,
    index=user_ids
)

similar_users = similar_users.drop(user_id)

voisins = similar_users.nlargest(5)

print(voisins)
```

**Sortie :** les cinq voisins les plus similaires.

### Estimer une note par moyenne pondérée

Exemple sur un livre non encore évalué par l'utilisateur cible :

```python
livre = mat_ratings.columns[0]

notes_voisins = mat_ratings.loc[
    voisins.index, livre
]

# Ne retenir que les voisins ayant effectivement noté le livre
mask = notes_voisins > 0

poids = voisins[mask]
notes = notes_voisins[mask]

if poids.abs().sum() > 0:
    note_predite = (
        (notes * poids).sum() / poids.abs().sum()
    )
else:
    note_predite = None
```

**Sortie :** estimation de la note décalée, ou `None` si aucune estimation n'est possible.

> [!warning] Interprétation
> Cette estimation simplifiée repose sur les notes disponibles et leur similarité.
>
> Les absences d'interactions ne doivent pas être considérées comme de véritables notes zéro.

---

## 4. Filtrage collaboratif — Item-based

**Question :** quels livres présentent des profils de notation similaires ?

### Transposer la matrice

```python
sparse_items = sparse_ratings.T.tocsr()
```

Avant : `utilisateurs × livres`.

Après : `livres × utilisateurs`.

### Calculer les similarités avec un livre

```python
livre = titles[0]
idx = titles.get_loc(livre)

scores = cosine_similarity(
    sparse_items[idx],
    sparse_items
).ravel()
```

### Sélectionner les livres proches

```python
similar_items = pd.Series(
    scores,
    index=titles
)

similar_items = similar_items.drop(livre)

print(similar_items.nlargest(10))
```

**Sortie :** livres dont les vecteurs de notations sont les plus similaires.

> [!important] Deux approches item-based différentes
> **Content-based :** compare les caractéristiques des livres.
>
> **Collaborative item-based :** compare les notes reçues des utilisateurs.

---

## 5. Factorisation matricielle — TruncatedSVD

**Objectif :** représenter les interactions dans un espace latent de dimension réduite.

### Créer et ajuster une SVD tronquée

Dans le troisième notebook, la réduction est appliquée aux livres, donc à la matrice transposée.

```python
from sklearn.decomposition import TruncatedSVD

svd = TruncatedSVD(
    n_components=12,
    random_state=42
)

ratings_red = svd.fit_transform(
    sparse_ratings.T
)

print(ratings_red.shape)
```

**Entrée :** matrice `livres × utilisateurs`.

**Sortie :** matrice `livres × 12 facteurs latents`.

`n_components=12` est ici le choix de l'exercice, pas une valeur universelle.

### Comparer les livres dans l'espace réduit

```python
idx = 0

scores = cosine_similarity(
    ratings_red[idx:idx+1],
    ratings_red
).ravel()

scores[idx] = -1

top_idx = scores.argsort()[::-1][:10]

print(titles[top_idx])
```

**Sortie :** dix livres proches selon leurs représentations latentes.

### Reconstruire une approximation

```python
matrice_reconstruite = svd.inverse_transform(
    ratings_red
)

print(matrice_reconstruite.shape)
```

**Sortie :** approximation dense de la matrice `livres × utilisateurs`.

> [!warning] Mémoire
> `inverse_transform` peut créer une grande matrice dense.
>
> La reconstruction n'est pas indispensable si l'on souhaite seulement calculer la similarité entre les vecteurs réduits.

---

## 6. Factorisation matricielle — NMF

**Objectif :** représenter une matrice non négative au moyen de facteurs eux-mêmes non négatifs.

Le cours présente le principe de NMF. L'implémentation ci-dessous sert de repère complémentaire avec scikit-learn.

```python
from sklearn.decomposition import NMF

nmf = NMF(
    n_components=12,
    init="nndsvda",
    max_iter=500,
    random_state=42
)

W = nmf.fit_transform(sparse_ratings)
H = nmf.components_

print(W.shape)
print(H.shape)
```

**Entrée :** matrice utilisateurs × livres, avec valeurs non négatives.

**Sorties :**
- `W` : utilisateurs × facteurs latents ;
- `H` : facteurs latents × livres.

### Approximation de la matrice initiale

```python
matrice_approximee = W @ H
```

> [!warning] Interprétation
> Cette reconstruction inclut des valeurs dans les cases sans interactions observées.
>
> Pour une véritable tâche de recommandation, il faut vérifier comment ces absences sont traitées pendant l'apprentissage et l'évaluation. Une factorisation NMF standard sur des zéros de remplissage ne reproduit pas automatiquement une méthode optimisée uniquement sur les notes observées.

---

## 7. Surprise — Modèle de prédiction des notes

**Objectif :** utiliser une bibliothèque spécialisée pour entraîner et évaluer des modèles de recommandation.

### Préparer les données

```python
from surprise import Dataset, Reader

reader = Reader(rating_scale=(0, 5))

data = Dataset.load_from_df(
    df_notes[["user_id", "title", "rating"]],
    reader
)
```

**Entrée :** DataFrame avec trois colonnes : utilisateur, élément, note.

**Sortie :** dataset reconnu par Surprise.

### Évaluer un modèle de référence

```python
from surprise import NormalPredictor
from surprise.model_selection import cross_validate

baseline = NormalPredictor()

resultats_baseline = cross_validate(
    baseline,
    data,
    measures=["RMSE", "MAE"],
    cv=5,
    verbose=True
)
```

`NormalPredictor` sert ici de référence simple.

### Évaluer SVD de Surprise

```python
from surprise import SVD

modele_svd = SVD(random_state=42)

resultats_svd = cross_validate(
    modele_svd,
    data,
    measures=["RMSE", "MAE"],
    cv=5,
    verbose=True
)
```

**Sortie :** scores de validation croisée.

Comparer les erreurs avec celles du modèle de référence.

> [!important] Attention aux deux SVD
> `sklearn.decomposition.TruncatedSVD` :
> réduction dimensionnelle explicite d'une matrice.
>
> `surprise.SVD` :
> algorithme spécialisé de factorisation pour prédire les notes utilisateur-produit.
>
> Ils ne s'utilisent pas de la même façon.

---

## 8. Entraîner le modèle final et obtenir des recommandations

### Ajuster sur toutes les interactions connues

À faire après avoir terminé le choix et l'évaluation du modèle.

```python
trainset = data.build_full_trainset()

modele_svd.fit(trainset)
```

### Générer les paires non encore évaluées

```python
anti_testset = trainset.build_anti_testset()
```

**Sortie :** liste de paires utilisateur-produit absentes des notes observées.

Ce n'est pas un jeu de test permettant de mesurer la RMSE : les véritables notes de ces paires ne sont pas connues.

### Prédire les notes

```python
predictions = modele_svd.test(anti_testset)
```

### Retrouver les meilleurs éléments pour un utilisateur

```python
user_id = df_notes["user_id"].iloc[0]

pred_user = [
    p for p in predictions
    if p.uid == user_id
]

top_10 = sorted(
    pred_user,
    key=lambda p: p.est,
    reverse=True
)[:10]

for p in top_10:
    print(p.iid, p.est)
```

**Sortie :** dix produits non notés, classés par notes prédites.

> [!warning] Volume
> `build_anti_testset()` génère toutes les paires non observées. Cette opération peut être très volumineuse.
>
> Pour une application réelle, il peut être préférable de générer uniquement les candidats de l'utilisateur ciblé.

---

## 9. Optimiser un modèle Surprise

### Définir la grille des paramètres

```python
from surprise.model_selection import GridSearchCV

param_grid = {
    "n_factors": [100, 150],
    "n_epochs": [20, 25, 30],
    "lr_all": [0.005, 0.01, 0.1],
    "reg_all": [0.02, 0.05, 0.1]
}
```

### Lancer la recherche

```python
grid = GridSearchCV(
    SVD,
    param_grid,
    measures=["rmse", "mae"],
    cv=5
)

grid.fit(data)
```

### Consulter les résultats

```python
print(grid.best_score["rmse"])
print(grid.best_params["rmse"])
```

**Sortie :** meilleure erreur de validation et hyperparamètres correspondants.

> [!warning] Temps de calcul
> La grille de l'exercice comporte de nombreuses combinaisons. Ne pas lancer une recherche coûteuse sans vérifier le nombre d'entraînements attendus.

---

## 10. Choix rapide de méthode

| Besoin | Méthode / outil |
|---|---|
| Comparer des descriptions | `TfidfVectorizer` + `cosine_similarity` |
| Comparer directement des utilisateurs | Similarité user-based |
| Comparer directement des produits selon les notes | Similarité item-based |
| Réduire la dimension des interactions | `TruncatedSVD` |
| Obtenir des facteurs non négatifs | `NMF` |
| Prédire les notes avec une bibliothèque spécialisée | `Surprise.SVD` |
| Comparer plusieurs hyperparamètres Surprise | `GridSearchCV` de Surprise |
| Associer plusieurs sources de recommandation | Approche hybride |

## 11. Points de vigilance transversaux

- Préserver la correspondance entre les identifiants, les index et les matrices.
- Ne pas confondre une interaction absente avec un avis négatif.
- Éviter les matrices de similarité complètes lorsque leur taille devient excessive.
- Distinguer similarité, note prédite et pertinence métier.
- Vérifier les recommandations obtenues et pas seulement les métriques numériques.
- Prévoir un traitement des nouveaux utilisateurs et des nouveaux produits (*cold start*).
- Préserver de la diversité pour éviter la surspécialisation.

---

