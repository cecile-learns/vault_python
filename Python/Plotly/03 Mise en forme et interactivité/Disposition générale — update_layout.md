# fig.update_layout

## Syntaxe

```python
fig.update_layout(title='Taille selon le sport', xaxis_title='Sport',
                  yaxis_title='Taille (cm)', showlegend=True,
                  legend_title='Sexe', width=800, height=600,
                  template='plotly_white')
```

## Paramètres

| Paramètre | Description |
|---|---|
| `title` ou `title_text` | Titre général. |
| `xaxis_title`, `yaxis_title` | Libellés des axes. |
| `showlegend` | Affiche ou masque la légende. |
| `legend_title` | Titre de légende. |
| `width`, `height` | Dimensions en pixels. |
| `autosize` | Adapte la taille au conteneur. |
| `template` | Thème, par exemple `'plotly_white'` ou `'plotly_dark'`. |
| `barmode` | `'group'`, `'stack'`, `'relative'` ou `'overlay'`. |
| `boxmode`, `violinmode` | `'group'` ou `'overlay'`. |
| `updatemenus` | Liste de boutons ou menus. Le cours utilise `type`, `buttons`, `method`, `args`, `label`, `x`, `y`. |

## Fonction

Modifie la disposition globale d’une Figure existante et renvoie cette figure.

## Vigilances & tips

- `barmode`, `boxmode` et `violinmode` concernent toute la figure.
- Un menu `updatemenus` demande une structure de dictionnaires : reprendre le modèle du cours.
- S’utilise souvent avec : `fig.add_trace()`, `fig.show()`.

## Documentation officielle

- [Plotly — Figure.update_layout](https://plotly.com/python-api-reference/generated/plotly.graph_objects.Figure.html#plotly.graph_objects.Figure.update_layout)
- [Plotly — boutons et menus](https://plotly.com/python/dropdowns/)
