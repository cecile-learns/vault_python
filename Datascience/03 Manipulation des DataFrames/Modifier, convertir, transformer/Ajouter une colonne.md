
1. Ajoutez une nouvelle colonne appelée `Experience` qui est calculée comme la différence (en années) entre 2024 et la `Join_Date`.
```python
df['Join_Date'] = pd.to_datetime(df['Join_Date'])
df['Experience'] = 2024 - df['Join_Date'].dt.year
display(df[['Name', 'Join_Date', 'Experience']])
```
