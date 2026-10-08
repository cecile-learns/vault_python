
# rotation

## Syntaxe

```python
def rotation_matrix(theta):
    A=np.array([[np.cos(theta),-np.sin(theta)],[np.sin(theta),np.cos(theta)]])
    return A
   
x=np.array([1,1])
A_r=rotation_matrix(np.pi).dot(x)

print(A_r)
```

## Fonction

Permet la rotation d'une matrice via un vecteur 

## Vigilances & tips

