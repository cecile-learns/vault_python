# chi2_contingency

## Syntaxe

```python
from scipy.stats import chi2_contingency

resultat_test = chi2_contingency(tableau_contingence)

statistique_chi2 = resultat_test.statistic
p_value = resultat_test.pvalue
degres_liberte = resultat_test.dof
effectifs_attendus = resultat_test.expected_freq
```

## Paramètres

- `tableau_contingence` : tableau des effectifs observés croisant les modalités de deux variables qualitatives.

## Fonction

Teste l’indépendance entre deux variables qualitatives. Le résultat fournit la statistique du chi-deux, la p-value, les degrés de liberté et les effectifs attendus sous l’hypothèse d’indépendance.

## Vigilances & tips

Contrôler les effectifs attendus : des effectifs trop faibles fragilisent l’approximation du test.

**S'utilise souvent avec :** `pd.crosstab()`.



``
```Python
# Solution

# Variable Policy_Sales_Channel

# On choisit d'utiliser les informations obtenues par le tableau de contingence pour
# réencoder la variable

train["Policy_Sales_Channel"].value_counts(
    normalize=True, dropna=False).head(5)

crosstab_policy = pd.crosstab(
    train["Policy_Sales_Channel"], train["Response"], normalize="index")

train['Policy_Sales_Channel'] = np.where(train['Policy_Sales_Channel'].isin(
    crosstab_policy[crosstab_policy[1] >= 0.12].index.tolist()), 1, 0)

# Test de Chi-Deux

# On vérifie la significativité de la variable

stat, p = chi2_contingency(pd.crosstab(
    train["Response"], train["Policy_Sales_Channel"]))[:2]

V_Cramer = np.sqrt(stat/pd.crosstab(train["Response"],
                                    train["Policy_Sales_Channel"]).values.sum())

# On affiche la liste des canaux de communication qui prennent la valeur 1

print()

print("La liste des codes des canaux de communication : \n\n",
      crosstab_policy[crosstab_policy[1] >= 0.12].index.tolist(), end="\n\n")

# On affiche la valeur du V de Cramer

print("Le V de Cramer est égal à : ", V_Cramer, end="\n\n")

# On affiche la p-valeur (pour trancher sur la significativité de la variable)

print("La p-valeur du test de Chi-Deux est égal à : ", p)
```

![[Pasted image 20261006152541.png]]

![[Pasted image 20261006152621.png]]

