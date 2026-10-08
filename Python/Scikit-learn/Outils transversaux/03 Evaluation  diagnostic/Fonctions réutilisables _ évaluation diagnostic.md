
## Quand l’utiliser ?

Après l’entraînement du modèle pour :

- mesurer les performances par classe ;
- comprendre les types d’erreurs ;
- repérer les zones d’incertitude ;
- identifier les variables importantes ;
- traduire les erreurs en coût ou enjeu métier.

> L’évaluation répond à **“combien le modèle est performant ?”**  
> Le diagnostic répond à **“où et comment se trompe-t-il ?”**

---



#  Métrique métier personnalisée — pattern

## Quand l’utiliser ?

Lorsque les erreurs n’ont **pas toutes le même coût métier**.

La Masterclass propose une fonction spécifique au cas RH : les faux négatifs concernant certains salariés sont davantage pénalisés. Masterclass ML

## Fonction du cours

```python
def compute_score(y, preds, alpha):

    maxi = np.where(
        alpha > 1,
        alpha,
        1
    ).sum()

    return (
        1
        - (
            (
                (preds > y).sum()
                + alpha[preds < y].sum()
            )
            / maxi
        )
    )
```

## Logique

```
bonne prédiction
→ pénalité 0

faux positif
→ pénalité fixe

faux négatif
→ pénalité pondérée selon alpha
```

Ici :

```
alpha = (    X["last_evaluation"]    * X["number_projects"])
```

Donc certaines erreurs coûtent davantage que d’autres.

## À retenir

Cette fonction **n’est pas générique**.

À conserver comme :

> **Pattern — construire une métrique personnalisée à partir du coût réel des erreurs.**

### Vigilance importante

Évaluer avec une métrique personnalisée n’est pas la même chose que :

```
entraîner le modèle avec ce coût
```

Si on veut que le coût métier influence directement l’apprentissage, il faut utiliser une stratégie adaptée : pondération, `sample_weight`, objectif personnalisé, etc.
