# Modifier, convertir, transformer

- [[Pandas/Modifier, convertir, transformer/replace|replace]]
- [[Pandas/Modifier, convertir, transformer/rename|rename]]
- [[Pandas/Modifier, convertir, transformer/drop|drop]]
- [[Pandas/Modifier, convertir, transformer/str.replace|str.replace]]
- [[Pandas/Modifier, convertir, transformer/astype|astype]]
- [[Pandas/Modifier, convertir, transformer/map|map]]
- [[Pandas/Modifier, convertir, transformer/apply|apply et lambda]]
- [[Pandas/Modifier, convertir, transformer/to_numeric|to_numeric]]
- [[Pandas/Modifier, convertir, transformer/to_datetime|to_datetime]]
- [[Pandas/Modifier, convertir, transformer/transform|transform]]
- [[Pandas/Modifier, convertir, transformer/round|round]]
- [[Pandas/Modifier, convertir, transformer/copy|copy]]
- [[Pandas/Modifier, convertir, transformer/Texte|Texte]]
- [[Ajouter une colonne]]
- [[str.fullmatch()]]
- [[pd.option_context]]
- [[Pandas/Modifier, convertir, transformer/pd.get_dummies|pd.get_dummies]]




Exemple du transformation de devises en une seul monnaie

```python
### On créée des séries distinctes contenant chacune les prix des produits dans chaque devise

price_AUD = df.loc[df['currency_buyer'] == 'AUD', 'price']
price_USD = df.loc[df['currency_buyer'] == 'USD', 'price']
price_GBP = df.loc[df['currency_buyer'] == 'GBP', 'price']
price_CHF = df.loc[df['currency_buyer'] == 'CHF', 'price']
price_EUR = df.loc[df['currency_buyer'] == 'EUR', 'price']

retail_AUD = df.loc[df['currency_buyer'] == 'AUD', 'retail_price']
retail_USD = df.loc[df['currency_buyer'] == 'USD', 'retail_price']
retail_GBP = df.loc[df['currency_buyer'] == 'GBP', 'retail_price']
retail_CHF = df.loc[df['currency_buyer'] == 'CHF', 'retail_price']
retail_EUR = df.loc[df['currency_buyer'] == 'EUR', 'retail_price']


### On convertit chaque série indépendamment 

price_AUD = price_AUD.apply(lambda x: conversion['AUD']*x)
price_USD = price_USD.apply(lambda x: conversion['USD']*x)
price_GBP = price_GBP.apply(lambda x: conversion['GBP']*x)
price_CHF = price_CHF.apply(lambda x: conversion['CHF']*x)

retail_AUD = retail_AUD.apply(lambda x: conversion['AUD']*x)
retail_USD = retail_USD.apply(lambda x: conversion['USD']*x)
retail_GBP = retail_GBP.apply(lambda x: conversion['GBP']*x)
retail_CHF = retail_CHF.apply(lambda x: conversion['CHF']*x)


### On remplace les anciennes colonnes 'price' et 'retail_price' par la concatenation des series obtenues

df["price"] = pd.concat([price_AUD, price_USD, price_GBP, price_CHF, price_EUR])

df["retail_price"] = pd.concat([retail_AUD, retail_USD, retail_GBP, retail_CHF, retail_EUR])

```