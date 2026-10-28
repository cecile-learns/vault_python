# Interpréter avec Captum

## Syntaxe rencontrée

```python
from captum.attr import LayerGradCam, Occlusion

grad_cam = LayerGradCam(model, couche_convolutive_cible)
attributions_gradcam = grad_cam.attribute(
    image_batch,
    target=indice_classe,
)

occlusion = Occlusion(model)
attributions_occlusion = occlusion.attribute(
    image_batch,
    target=indice_classe,
    sliding_window_shapes=(3, 15, 15),
    strides=(3, 8, 8),
)
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| couche cible | couche expliquée | dernière couche convolutive en général |
| `target` | classe expliquée | indice réel ou prédit selon l'analyse |
| `sliding_window_shapes` | taille de la zone masquée | plus grande = carte plus grossière |
| `strides` | déplacement de la fenêtre | plus petit = plus précis mais plus coûteux |

## Vigilances & tips

Une attribution explique localement le modèle ; elle ne démontre pas une relation causale.
