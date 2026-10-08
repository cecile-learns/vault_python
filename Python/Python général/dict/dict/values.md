# values

## Syntaxe

```python
valeurs = dictionnaire.values()
```

## Fonction

Retourne une vue des valeurs.

```python
person = {"name": "Alice", "age": 25, "city": "New York"}

valeurs=person.values()
print(valeurs)
```

dict_values(['Alice', 25, 'New York'])

```python
scores = {"Alice" : 90, 'Bob': 85, 'Charlie' : 92}
valeurs=scores.values()
print(valeurs)

dict_values([90, 85, 92])
```