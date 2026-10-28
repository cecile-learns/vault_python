# Lisser — `rolling().mean()`

## Pattern

```python
moyenne_mobile = serie.rolling(window=12, center=True).mean()
```

## Paramètres utiles

- `window` : taille de la fenêtre ;
- `center=True` : place le résultat au centre de la fenêtre ;
- `min_periods` : nombre minimal d'observations nécessaires.

## Interprétation

Une moyenne mobile atténue les variations courtes et aide à voir la tendance. Une fenêtre égale à la période saisonnière peut absorber une grande partie de cette saisonnalité.

## Vigilance

Une fenêtre centrée utilise aussi des valeurs futures : elle convient à l'analyse descriptive, mais pas à la fabrication d'une variable disponible en temps réel.

[Documentation officielle — `DataFrame.rolling`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.rolling.html)


### Exemple de cours
```python
#On a une saisonnalité de période 12

airpass_ma = airpasslog.rolling(window = 12, center = True).mean()

#Affichage de la série 

plt.plot(airpasslog, color = 'blue', label = 'Origine')
plt.plot(airpass_ma, color = 'red', label = 'Moyenne mobile')
plt.legend()
plt.title('Méthode des moyennes mobiles')
plt.show()

```
![[Pasted image 20261017094950.png]]