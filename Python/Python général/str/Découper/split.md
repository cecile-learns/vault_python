# split

## Syntaxe

```python
mots = texte.split(' ')
```

## Paramètres

Le séparateur est facultatif ; sans argument, les espaces successifs sont regroupés.

## Fonction

Découpe une chaîne et retourne une liste.

```python
def word_count(sentence):
    words = sentence.split()
    word_freq = {}
    for word in words:
        word_freq[word] = word_freq.get(word, 0) + 1
    return word_freq

sentence = "this is a test this is only a test"
print(word_count(sentence))  # Sortie : {'this': 2, 'is': 2, 'a': 2, 'test': 2, 'only': 1}
```