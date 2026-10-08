# compréhension

## Syntaxe

```python
carres = {n: n ** 2 for n in nombres}
```

## Fonction

Construit un dictionnaire par compréhension.

```python
person = {"name": "Alice", "age": 25, "city": "New York"}

print(person['name'])
print(person['city'])
```


```python
scores = {"Alice" : 90, 'Bob': 85, 'Charlie' : 92}
for person, score in scores.items():
      print(f"{person} : {score}")
```

#
```python
def create_grade_dict (students, grades):
    return {students[i] : grades[i] for i in range(len(students))}

students = ["Alice", "Bob", "Charlie"]
grades = [85, 90, 78]
print (create_grade_dict (students, grades))
```

