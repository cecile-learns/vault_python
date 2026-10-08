

```python
def add_numbers(a,b):
    return (a+b)

resultat = add_numbers(2,3)
print (resultat)

# b
def max_of_three(x, y, z):
    maxi=0
    if x>y and x>z:
        maxi=x
    elif y>x and y>z:
        maxi=y
    else:
        maxi=z
    return maxi

result=max_of_three(1,2,3)
print(result)
#a noter dans la correction utilisation de return max(x,y,z) 

#c
def is_palindrome(word):
    return word == word[::-1]
print(is_palindrome("madam"))  
print(is_palindrome("hello")) 

#d
def convert_temperature(celsius):
    return (celsius * 9/5) + 32
print(convert_temperature(25))


```
