
![[Pasted image 20261006154202.png]]

```Python
scaler1 = MinMaxScaler()
scaler2 = RobustScaler()

train[['Age', 'Vintage']] = scaler1.fit_transform(
    train[['Age', 'Vintage']])
    
train['Annual_Premium'] = scaler2.fit_transform(
    train[['Annual_Premium']])
```

