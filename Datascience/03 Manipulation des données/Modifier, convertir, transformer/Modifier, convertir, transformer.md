# Modifier, convertir, transformer

- [[Manipulation des données/Modifier, convertir, transformer/replace|replace]]
- [[Manipulation des données/Modifier, convertir, transformer/rename|rename]]
- [[Manipulation des données/Modifier, convertir, transformer/drop|drop]]
- [[Manipulation des données/Modifier, convertir, transformer/str.replace|str.replace]]
- [[Manipulation des données/Modifier, convertir, transformer/astype|astype]]
- [[Manipulation des données/Modifier, convertir, transformer/map|map]]
- [[Manipulation des données/Modifier, convertir, transformer/apply|apply et lambda]]
- [[Manipulation des données/Modifier, convertir, transformer/to_numeric|to_numeric]]
- [[Manipulation des données/Modifier, convertir, transformer/to_datetime|to_datetime]]
- [[Manipulation des données/Modifier, convertir, transformer/transform|transform]]
- [[Manipulation des données/Modifier, convertir, transformer/round|round]]
- [[Manipulation des données/Modifier, convertir, transformer/copy|copy]]
- [[Manipulation des données/Modifier, convertir, transformer/Texte|Texte]]
- [[Manipulation des données/Modifier, convertir, transformer/Ajouter une colonne]]
- [[Manipulation des données/Modifier, convertir, transformer/str.fullmatch()]]
- [[Manipulation des données/Modifier, convertir, transformer/pd.option_context]]
- [[Manipulation des données/Modifier, convertir, transformer/pd.get_dummies|pd.get_dummies]]




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

---

## Navigation du second Vault

- [[Manipulation des données/Modifier, convertir, transformer/Ajouter une colonne|Ajouter une colonne]]
- [[Manipulation des données/Modifier, convertir, transformer/apply|apply]]
- [[Manipulation des données/Modifier, convertir, transformer/astype|astype]]
- [[Manipulation des données/Modifier, convertir, transformer/copy|copy]]
- [[Manipulation des données/Modifier, convertir, transformer/drop|drop]]
- [[Manipulation des données/Modifier, convertir, transformer/map|map]]
- [[Manipulation des données/Modifier, convertir, transformer/pd.get_dummies|pd.get_dummies]]
- [[Manipulation des données/Modifier, convertir, transformer/pd.option_context|pd.option_context]]
- [[Manipulation des données/Modifier, convertir, transformer/rename|rename]]
- [[Manipulation des données/Modifier, convertir, transformer/replace|replace]]
- [[Manipulation des données/Modifier, convertir, transformer/round|round]]
- [[Manipulation des données/Modifier, convertir, transformer/str.fullmatch()|str.fullmatch()]]
- [[Manipulation des données/Modifier, convertir, transformer/str.replace|str.replace]]
- [[Manipulation des données/Modifier, convertir, transformer/Texte|Texte]]
- [[Manipulation des données/Modifier, convertir, transformer/to_datetime|to_datetime]]
- [[Manipulation des données/Modifier, convertir, transformer/to_numeric|to_numeric]]
- [[Manipulation des données/Modifier, convertir, transformer/transform|transform]]
