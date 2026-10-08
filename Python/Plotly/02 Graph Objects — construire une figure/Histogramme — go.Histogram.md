# go.Histogram

## Syntaxe

```python
trace = go.Histogram(x=df_livres['user_rating'], nbinsx=20,
                     histnorm='probability density', marker_color='lightgreen',
                     opacity=0.7, name='Échantillon')
fig.add_trace(trace)
fig.update_layout(barmode='overlay')
```

## Paramètres

| Paramètre      | Description                                                                                   |
| -------------- | --------------------------------------------------------------------------------------------- |
| `x`            | Valeurs à regrouper en classes.                                                               |
| `nbinsx`       | Nombre approximatif de classes.                                                               |
| `histnorm`     | Normalisation : `'percent'`, `'probability'` ou `'probability density'`.                      |
| `marker_color` | Couleur des barres.                                                                           |
| `marker_line`  | Dictionnaire du contour.                                                                      |
| `opacity`      | Opacité, utile pour une superposition. de 0 à 1                                               |
| `name`         | Nom de légende.                                                                               |
| `histfunc`     | `'count'`, `'sum'`, `'avg'`, `'min'` ou `'max'`.                                              |
| `barmode`      | Paramètre de `update_layout` : `'overlay'`( superpose), `'group'`, `'stack'` ou `'relative'`. |
- **"stack"** : Les barres sont situées l'une au dessus de l'autre.
    
- **"group"** : Les barres sont situées l'une à côté de l'autre centrée autour de leur valeur en commun.
    
- **"relative"** : Les barres sont empilées les unes sur les autres, les valeurs négatives étant situées en dessous de l'axe, les valeurs positives au-dessus.
    
- **"overlay"** : Les barres sont superposées.
## Fonction

Crée un tracé d’histogramme à ajouter à une Figure.

## Vigilances & tips

- `nbinsx` influence fortement la lecture.
- Pour comparer des distributions superposées, combiner `opacity` et `barmode='overlay'`.

## Documentation officielle

- [Plotly — go.Histogram](https://plotly.com/python-api-reference/generated/plotly.graph_objects.Histogram.html)
```Python

df_2009=data.loc[data['Year']== 2009]
df_2014=data.loc[data['Year']== 2014]
df_2019=data.loc[data['Year']== 2019]

fig = go.Figure()


fig.add_trace(go.Histogram(x = df_2009['Price'], 
                           name='2009',
                          opacity=0.25))

fig.add_trace(go.Histogram(x = df_2014['Price'], 
                           name='2014',
                          opacity=0.5))

fig.add_trace(go.Histogram(x = df_2019['Price'], 
                           name='2019',
                          opacity=0.75))



fig.update_layout(title = "Comparaison de la distribution des tarifs",
                  xaxis_title = 'Valeur',
                  yaxis_title = 'Tarif',
                  barmode='overlay')

fig.show("notebook")
```