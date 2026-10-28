

Application de transformation aléatoires aux images du train pour augmenter artificiellement la taille et la diversité du dataset

```python
# mini-pipeline d'augmentation
data_augmentation = models.Sequential([
    layers.RandomFlip("horizontal"),
    layers.RandomRotation(0.1),
    layers.RandomZoom(0.1),
    layers.RandomTranslation(0.1, 0.1),
], name="data_augmentation")

# 1 image originale vs 8 versions augmentées
sample_img = X_train_norm[0:1]  # Shape (1, 32, 32, 3)

fig, axes = plt.subplots(1, 9, figsize=(18, 3))
axes[0].imshow(sample_img[0])
axes[0].set_title("Original", fontsize=10, fontweight="bold")
axes[0].axis("off")

for i in range(1, 9):
    augmented = data_augmentation(sample_img, training=True)
    axes[i].imshow(augmented[0].numpy())
    axes[i].set_title(f"Aug. {i}", fontsize=10)
    axes[i].axis("off")

plt.tight_layout()
plt.show()
```

![[Pasted image 20261026102630.png]]




