# savefig

> [!info] Bibliothèque
> Matplotlib

## Syntaxe

```python
plt.savefig("graphique.png", dpi=150, bbox_inches="tight")
```

## Paramètres

Nom : chemin de sortie et extension ; `dpi` : résolution ; `bbox_inches="tight"` : ajuster le cadre aux éléments.

## Fonction

Enregistre la figure courante.

## Vigilances & tips

À placer avant `plt.show()`. Un chemin relatif dépend du dossier courant ; choisir une destination autorisée. Un fichier de même nom peut être remplacé. Dans Colab, le fichier reste dans la session jusqu’à son téléchargement.

Source : Liora, cours Matplotlib ; ressources personnelles de visualisation.

## Paramètres courants complémentaires

| Paramètre | Effet |
|---|---|
| `format` | Format de sortie explicite, par exemple `'png'`, `'pdf'` ou `'svg'`; sinon l’extension du fichier est utilisée. |
| `transparent=True` | Rend transparent le fond de la figure et des Axes. |
| `facecolor` | Couleur de fond du fichier exporté. |
| `pad_inches` | Marge autour de la figure, en pouces, notamment avec `bbox_inches='tight'`. |
| `metadata` | Métadonnées prises en charge par certains formats. |

## Documentation officielle

[Matplotlib — `pyplot.savefig`](https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.savefig.html)
