# Modifier, convertir, transformer





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

## Navigation

- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/Ajouter une colonne|Ajouter Une Colonne]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/Shift|Shift]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/Texte|Texte]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/apply|Apply]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/astype|Astype]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/copy|Copy]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/drop|Drop]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/map|Map]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/pd.get_dummies|Pd.Get_Dummies]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/pd.option_context|Pd.Option_Context]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/rename|Rename]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/replace|Replace]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/round|Round]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/str.fullmatch()|Str.Fullmatch()]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/str.replace|Str.Replace]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/to_datetime|To_Datetime]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/to_numeric|To_Numeric]]
- [[03 Manipulation des DataFrames/Modifier, convertir, transformer/transform|Transform]]
