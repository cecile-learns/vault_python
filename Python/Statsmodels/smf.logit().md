
# `smf.logit()`

## Quand l’utiliser ?

Utiliser `smf.logit()` pour ajuster une **régression logistique avec `statsmodels`** lorsqu’on veut surtout :

- interpréter les coefficients ;
- tester leur significativité ;
- obtenir les p-values et intervalles de confiance ;
- étudier le lien entre une cible binaire et une ou plusieurs variables explicatives.

## Principe

La syntaxe utilise une **formule** :

```
smf.logit("y ~ x", data=df)
```

Se lit :

```
y expliquée par x
```

## Pattern minimal

```python
import statsmodels.formula.api as smf
logit = smf.logit( "y ~ x", data=df).fit()
logit.summary2()
```

## Plusieurs variables explicatives

```python
logit = smf.logit("y ~ x1 + x2 + x3",data=df).fit()logit.summary2()
```

Exemple :

```python
logit = smf.logit("Response ~ Age + Gender + Year",data=train).fit()
```

---

## Paramètres
`.fit()` : ajuste le modèle sur les données. sans `.fit()`, le modèle est seulement défini, pas encore estimé.
`summary2()` : Affiche notamment :

- coefficients ;
- erreurs standards ;
- statistique `z` ;
- p-values ;
- intervalles de confiance.

## Lire la p-value

Repère utilisé dans le cours :

```
p-value < 0.05
→ coefficient statistiquement significatif

p-value >= 0.05
→ pas de preuve suffisante de relation statistique
```

Attention : significatif ne veut pas dire automatiquement **important** ni **utile métier**.

## Lire le signe du coefficient

```
coefficient > 0
→ quand x augmente,
  la probabilité de y = 1 tend à augmenter

coefficient < 0
→ quand x augmente,
  la probabilité de y = 1 tend à diminuer
```

## Différence avec `LogisticRegression`

```
statsmodels — smf.logit()
→ interprétation statistique
→ coefficients
→ p-values
→ intervalles de confiance

sklearn — LogisticRegression()
→ prédiction
→ évaluation
→ pipeline ML
→ validation croisée
```

## Points de vigilance

- La cible doit être binaire.
- Les noms utilisés dans la formule doivent correspondre aux colonnes du DataFrame.
- `p < 0.05` ne prouve pas une relation causale.
- Une variable non significative seule peut parfois être utile dans un modèle multivarié.
- La significativité statistique ne remplace pas l’évaluation prédictive du modèle.

## À retenir

```
smf.logit()
→ régression logistique avec statsmodels

"y ~ x1 + x2"
→ formule du modèle

.fit()
→ ajuste le modèle

.summary2()
→ coefficients + p-values + diagnostic statistique
```

