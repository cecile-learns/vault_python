# Décomposer — `seasonal_decompose`

## Pour faire quoi ?

Séparer visuellement une série en tendance, saisonnalité et résidu.

## Pattern

```python
from statsmodels.tsa.seasonal import seasonal_decompose

decomposition = seasonal_decompose(
    serie,
    model="multiplicative",
    period=12,
)
decomposition.plot()

#Exemple de cours
res = seasonal_decompose(airpass, model = 'multiplicative')
res.plot()
plt.show()

```
![[Pasted image 20261017095119.png]]
## Paramètres utiles

- `model="additive"` ou `"multiplicative"` ;
- `period` : nombre d'observations dans un cycle saisonnier ;
- `extrapolate_trend` : extrapolation de la tendance aux extrémités.

## Vigilances

La série doit avoir une fréquence cohérente et contenir au moins deux cycles complets. Examiner le résidu : une structure encore visible signale que la décomposition n'explique pas tout.

[Documentation officielle — `seasonal_decompose`](https://www.statsmodels.org/stable/generated/statsmodels.tsa.seasonal.seasonal_decompose.html)


### Exemple du cours
```python
import numpy as np
airpasslog=np.log(airpass)
plt.plot(airpasslog)
plt.show()

# On applique la fonction seasonal_decompose à airpasslog 
# Ici on utilise la transformée en log pour avoir un modèle additif

mult = seasonal_decompose(airpasslog)

# On soustrait les coefficients saisonniers à la série airpasslog

cvs = airpasslog['#Passengers'] - mult.seasonal

# On applique la fonction exponentielle pour retrouver la série originale

x_cvs = np.exp(cvs)

# On affiche la série

plt.plot(airpass, label='Série originale')
plt.plot(x_cvs, label='Série corrigée') 

plt.title('Graphique de la série originale et la série corrigée')
plt.xlabel('Date')
plt.ylabel('Nb passagers')
plt.legend()
plt.show();
```
![[Pasted image 20261017093111.png]]