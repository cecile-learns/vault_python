# Accueil

## Navigation

- [[README — organisation|Readme — Organisation]]
- [[01 Python général/Python général|Python Général]]
- [[02A Manipulation des arrays/Manipulation des arrays|Manipulation Des Arrays]]
- [[02B Manipulation des tenseurs/Manipulation des tenseurs|Manipulation Des Tenseurs]]
- [[03 Manipulation des DataFrames/Manipulation des DataFrames|Manipulation Des Dataframes]]
- [[04 Dataviz/Dataviz|Dataviz]]
- [[05 Préparation et traitements spécialisés/Préparation et traitements spécialisés|Préparation Et Traitements Spécialisés]]
- [[06 Modélisation ML/Modélisation|Modélisation]]
- [[07 Deep Learning/Deep Learning|Deep Learning]]
- [[08 Automatisation/Automatisation|Automatisation]]
- [[09 Présentation des résultats/Présentation des résultats|Présentation Des Résultats]]


```python
import base64
from IPython.display import HTML, display

csv = df.to_csv(index=False)
b64 = base64.b64encode(csv.encode("utf-8")).decode("utf-8")

display(HTML(
    f'<a download="mushrooms.csv" href="data:text/csv;base64,{b64}">'
    '📥 Télécharger mushrooms.csv</a>'
))
```
