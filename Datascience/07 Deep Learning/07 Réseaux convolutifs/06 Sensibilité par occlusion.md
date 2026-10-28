# Sensibilité par occlusion

## Quand l'utiliser ?

Masquer des zones et mesurer la baisse du score.

## Pattern

```python
import numpy as np
def occlusion_sensitivity(model, image, target_class_index, patch_size=32, stride=16):
    img_height, img_width = image.shape[1], image.shape[2]
    
    # Probabilité initiale sans masque
    initial_preds = model.predict(image, verbose=0)
    initial_prob = initial_preds[0, target_class_index]
    
    heatmap = np.zeros((img_height, img_width))
    counts = np.zeros((img_height, img_width))

    # Parcours spatial avec la fenêtre (masque)
    for y in range(0, img_height - patch_size + 1, stride):
        for x in range(0, img_width - patch_size + 1, stride):
            # Créer une image où la zone visée est occultee (remplacée par la moyenne ImageNet par exemple)
            occluded_image = np.copy(image)
            occluded_image[:, y:y+patch_size, x:x+patch_size, :] = 0.0 # Masquage "noir" (selon preprocess)
            
            # Prédire avec le masque
            preds = model.predict(occluded_image, verbose=0)
            prob = preds[0, target_class_index]
            
            # La baisse de probabilité indique l'importance de la zone : (initial_prob - prob)
            heatmap[y:y+patch_size, x:x+patch_size] += (initial_prob - prob)
            counts[y:y+patch_size, x:x+patch_size] += 1

    # Moyenne pour les pixels superposés
    counts = np.maximum(counts, 1)
    heatmap = heatmap / counts
    heatmap = np.maximum(heatmap, 0)
    heatmap = heatmap / np.max(heatmap) # Normalisation 0-1
    return heatmap

# Calcul de la carte d'occlusion (cela peut prendre quelques dizaines de secondes !)
# L'index 386 correspond grossièrement à l'éléphant selon VGG16 sur l'image
dog_index = tf.argmax(model_vgg.predict(img_preprocessed)[0]).numpy()
heatmap_occ = occlusion_sensitivity(model_vgg, img_preprocessed, dog_index, patch_size=60, stride=30)
```
![[Pasted image 20261026112010.png]]
## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `taille` | taille du masque |
| `pas` | espacement des tests |

## Points de vigilance

- Méthode coûteuse en prédictions.
- La valeur masquée influence le résultat.

## Documentation officielle

- [Référence](https://keras.io/examples/vision/)
