# loc

## Syntaxe

```python
lignes_adultes = df.loc[df['col_age'] >= 18, ['col_nom', 'col_age']]


df.loc[df['Department'] == 'Sales', 'Performance_Score'] += 5
# ne pas mettre df['Performance_Score']=

# Filtrer les lignes 
resultat = df.loc[condition] #identitique à resultat=df[condition]
# Filtrer lignes + colonnes 
resultat = df.loc[condition, "colonne"] 
# Modifier sous condition 
df.loc[condition, "colonne"] = nouvelle_valeur
```
Attention si plusieurs conditions il faut penser aux parenthèses entre les condition. il faut aussi penser à rappeler df
## Paramètres

`df['col_age'] >= 18` : condition qui sélectionne les lignes.

`['col_nom', 'col_age']` : colonnes conservées dans le résultat.

## Fonction

Sélectionne des lignes et des colonnes à partir de labels ou d’un masque booléen.

## Vigilances & tips

La borne de fin d’une tranche de labels est incluse.
