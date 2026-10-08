# despine

## Pour faire quoi ?

Retirer certaines bordures d’un graphique pour alléger sa présentation.

## Syntaxe réutilisable

```python
sns.despine(ax=ax, top=True, right=True, left=False, bottom=False, offset=5, trim=False)
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `fig` | Figure Matplotlib concernée. |
| `ax` | Axes précis à modifier. |
| `top` | Retire la bordure supérieure si `True`. |
| `right` | Retire la bordure droite si `True`. |
| `left` | Retire la bordure gauche si `True`. |
| `bottom` | Retire la bordure inférieure si `True`. |
| `offset` | Décale les bordures restantes vers l’extérieur, en points. |
| `trim` | Limite les bordures aux graduations extrêmes si `True`. |

## Fonction

Modifie les bordures de l’Axes ou de la figure existante et ne renvoie pas un nouveau graphique.

## Vigilances & tips

- Si ni `fig` ni `ax` ne sont fournis, la fonction agit sur la figure courante.
- Conserver les bordures qui servent à lire l’échelle.
- Appliquer après la création du graphique.

## Documentation officielle

[Seaborn — `despine`](https://seaborn.pydata.org/generated/seaborn.despine.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
