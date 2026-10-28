# Grad-CAM

## Quand l'utiliser ?

Localiser les zones qui influencent une classe.

## Pattern

```python
import tensorflow as tf
def make_gradcam_heatmap(img_array, model, last_conv_layer_name, pred_index=None):
    # Création d'un sous-modèle qui renvoie l'activation de la dernière couche
    # et la prédiction finale.
    grad_model = tf.keras.models.Model(
        [model.inputs], 
        [model.get_layer(last_conv_layer_name).output, model.output]
    )

    with tf.GradientTape() as tape:
        last_conv_layer_output, preds = grad_model(img_array)
        if pred_index is None:
            pred_index = tf.argmax(preds[0])
        # Le "loss" retenu est le logit (valeur d'activation) de la classe prédite
        class_channel = preds[:, pred_index]

    # Dérivée de la perte par rapport à la dernière couche conv (les gradients)
    grads = tape.gradient(class_channel, last_conv_layer_output)

    # Réduction pondérée des gradients par canal (Global Average Pooling des gradients sur X et Y)
    pooled_grads = tf.reduce_mean(grads, axis=(0, 1, 2))

    # On multiplie chaque canal de l'activation par le gradient moyen correspondant
    last_conv_layer_output = last_conv_layer_output[0]
    heatmap = last_conv_layer_output @ pooled_grads[..., tf.newaxis]
    heatmap = tf.squeeze(heatmap)

    # On passe la heatmap dans une ReLU pour ne garder que l'influence positive
    heatmap = tf.maximum(heatmap, 0) / tf.math.reduce_max(heatmap)
    return heatmap.numpy()

# Pour VGG16, la dernière couche de conv s'appelle "block5_conv3"
heatmap = make_gradcam_heatmap(img_preprocessed, model_vgg, last_conv_layer_name='block5_conv3')

plt.matshow(heatmap)
plt.title("Heatmap Grad-CAM brute (basse résolution)")
plt.colorbar()
plt.show()
```
![[Pasted image 20261026111904.png]]

```python
def save_and_display_gradcam(img_path, heatmap, alpha=0.4):
    # image originale
    img = img_to_array(load_img(img_path))

    # Transformer la heatmap en couleurs
    heatmap = np.uint8(255 * heatmap)
    jet = cm.get_cmap("jet")
    
    # Récupérer les valeurs RGB de la couleur "jet" (exclure le canal alpha)
    jet_colors = jet(np.arange(256))[:, :3]
    jet_heatmap = jet_colors[heatmap]

    # Redimensionnement au format de l'image de base
    jet_heatmap = tf.keras.utils.array_to_img(jet_heatmap)
    jet_heatmap = jet_heatmap.resize((img.shape[1], img.shape[0]))
    jet_heatmap = tf.keras.preprocessing.image.img_to_array(jet_heatmap)

    # Superposer l'image de base et la heatmap colorée
    superimposed_img = jet_heatmap * alpha + img
    superimposed_img = tf.keras.utils.array_to_img(superimposed_img)

    plt.figure(figsize=(8,8))
    plt.imshow(superimposed_img)
    plt.axis('off')
    plt.show()

save_and_display_gradcam('test_image.jpg', heatmap)
```

![[Pasted image 20261026111946.png]]

## Paramètres et éléments utiles

| Élément | Rôle |
|---|---|
| `conv_layer_name` | couche convolutive |
| `class_index` | classe expliquée |

## Points de vigilance

- L'explication est locale au modèle et à l'image.

## Documentation officielle

- [Référence](https://keras.io/examples/vision/grad_cam/)
