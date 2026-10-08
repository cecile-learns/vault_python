# recherche par booléen

## Syntaxe

```python
valeurs = tableau[tableau > 0]
```

## Fonction

Sélectionne les valeurs qui vérifient une condition.



```python
items_90=items[discounts==90]
quantites_90=quantities[discounts==90]

print(items_90,quantites_90)
```
['needle' 'bag' 'perfume' 'fake flowers' 'watch'] [914 504 298 291  15]


```python
print("Réduction pour le téléphone : ",discounts[items=='cell phone'])

print("Réduction pour le speakers :", discounts[items=='speakers'])
```
​

Réduction pour le téléphone :  [25]
Réduction pour le speakers : [50]

```python
faible_quantite = quantities < 50

for item, discount in zip(items[faible_quantite], discounts[faible_quantite]):
    print(f"{item} : {discount}")
```
piano : 25
scotch tape : 50
watch : 90
sticky note : 50
