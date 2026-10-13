# set_theme

> [!info] Bibliothèque
> Seaborn

## Pour faire quoi ?

Appliquer un thème global à tous les graphiques Seaborn et Matplotlib créés ensuite.

## Syntaxe réutilisable

```python
sns.set_theme(context='notebook', style='whitegrid', palette='deep', font_scale=1.0)
# sns.set(...)  # alias historique de set_theme
```

## Paramètres courants

| Paramètre | Description |
|---|---|
| `context` | Échelle générale : `'paper'`, `'notebook'`, `'talk'` ou `'poster'`. |
| `style` | Fond et grille : `'darkgrid'`, `'whitegrid'`, `'dark'`, `'white'` ou `'ticks'`. |
| `palette` | Cycle de couleurs Seaborn ou Matplotlib. |
| `font` | Nom de la police. |
| `font_scale` | Facteur multiplicatif de la taille des textes. |
| `color_codes` | Réinterprète les codes de couleurs abrégés si `True`. |
| `rc` | Dictionnaire de paramètres Matplotlib supplémentaires. |

## Fonction

Modifie les réglages graphiques globaux. `sns.set()` est un alias ; `sns.set_theme()` est l’interface recommandée.

## Vigilances & tips

- Appeler la fonction avant les tracés concernés.
- Le réglage reste actif pour les figures suivantes de la session.
- Utiliser un dictionnaire `rc` seulement pour des ajustements Matplotlib précis.

## Documentation officielle

[Seaborn — `set_theme`](https://seaborn.pydata.org/generated/seaborn.set_theme.html)

Sources : ressources Seaborn du workspace ; documentation officielle Seaborn.
