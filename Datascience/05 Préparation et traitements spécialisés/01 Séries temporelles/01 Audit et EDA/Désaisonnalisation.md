
## Workflow du cours

```python
#Affecter à la variable `airpasslog_without_ma` la valeur de la **différence** entre la série 𝑋𝑡 et sa moyenne mobile 𝑀12𝑋𝑡
airpasslog_without_ma=airpasslog - airpass_ma

# Supprimer les valeurs manquantes de la série **`airpasslog_without_ma`**
airpasslog_without_ma = airpasslog_without_ma.dropna()

#Stocker la serie dans un dataframe, ajouter la colonne month à l'aide de l'attribut index
df=airpasslog_without_ma
df['month'] = (df.index).month

#Calculer le coefficient saisonnier lié à chaque mois et le centrer
seasonality = df.groupby('month').mean()
seasonality = seasonality - seasonality.mean()

#Créer une serie corrigée de ses variations saisonnières
# Création du vecteur 
seasonal_vector = np.zeros(144)
j=0
for i in range(12):
    seasonal_vector[j:j+12] = seasonality.iloc[:,0]
    j=j+12
# Soustraire les coefficients saisonniers
airpasslog_cvs = airpasslog["#Passengers"] - seasonal_vector
# On retrouve la série originale par passage à l'exponentielle
airpass_cvs = np.exp(airpasslog_cvs)

#Visualiser la serie 
plt.plot(airpass, ':', label = '$X_t$')
plt.plot(airpass_cvs, label = '$X_t^{CVS}$')
plt.title('Série originale et corrigée de ses variations saisonnières')
plt.xlabel('t')
plt.ylabel('# Passengers')
plt.legend();
```
![[Pasted image 20261017101312.png]]

On peut lire sur la courbe 𝑋𝐶𝑉𝑆𝑡 des chutes de fréquentation aux années 1958 et 1960 qu'il aurait été impossible de déceler sur la courbe originale. Voici l'un des intérêts de la désaisonnalisation d'une série temporelle.