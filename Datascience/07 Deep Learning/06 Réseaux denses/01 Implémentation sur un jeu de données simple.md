
```python
import tensorflow as tf
print(tf.__version__)

import pandas as pd

df=pd.read_csv('iris.csv', index_col = 'Id')
df.head()
```
![[Pasted image 20261024113936.png]]

``
```Python
import numpy as np
# Séparation des variables 
X = df[['SepalLengthCm', 'SepalWidthCm', 'PetalLengthCm', 'PetalWidthCm']].values
y = df['Species'].values

#Prétraitement
from sklearn.preprocessing import LabelEncoder
from tensorflow.keras.utils import to_categorical

# Encodage : string -> int -> one-hot
label_encoder = LabelEncoder()
y_int = label_encoder.fit_transform(y)          # ex: Iris-setosa -> 0, ...
num_classes = len(np.unique(y_int))             # ici 3
y_cat = to_categorical(y_int, num_classes=num_classes) # représentation one-hot

#Séparation du jeu d'entraînement 
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y_cat, test_size=0.2, random_state=42, stratify=y_cat

# Construction des couches
from tensorflow.keras import Sequential, layers

model_seq = Sequential([
    layers.Input(shape=(4,)),
    layers.Dense(64, activation='relu'),
    layers.Dense(num_classes, activation='softmax')
])

#Compilation
from tensorflow.keras.optimizers import SGD

model_seq.compile(
    optimizer=SGD(learning_rate=1e-3),
    loss='categorical_crossentropy',
    metrics=['accuracy']
)

# Entrainement
history = model_seq.fit(
    X_train, y_train,
    batch_size=16,
    epochs=50,
    verbose=1
)
```
![[Pasted image 20261024114222.png]]

```python
# Evaluation
test_loss, test_acc = model_seq.evaluate(X_test, y_test, verbose=0)
print(f"Test loss: {test_loss:.2f}\nTest accuracy: {test_acc:.2f}")

y_proba = model_seq.predict(X_test)            # distributions de probas
print(y_proba)
```
![[Pasted image 20261024114331.png]]

```python
# Evaluation suite
test_loss, test_acc = model_seq.evaluate(X_test, y_test, verbose=0)
print(f"Test accuracy: {test_acc:.2f}")

y_proba = model_seq.predict(X_test)            # distributions de probas
y_pred_int = np.argmax(y_proba, axis=1)        # classes prédites (0..2)
y_true_int = np.argmax(y_test, axis=1)
```
Test accuracy: 0.83
1/1 [==============================] - 0s 13ms/step

```python
#Matrice de confusion
from sklearn.metrics import confusion_matrix
import seaborn as sns
import matplotlib.pyplot as plt

cm = confusion_matrix(y_true_int, y_pred_int)

# revenir aux labels textuels (optionnel, utile pour + de lisibilité)
y_pred_labels = label_encoder.inverse_transform(y_pred_int)
y_true_labels = label_encoder.inverse_transform(y_true_int)

plt.figure(figsize=(6, 5))
sns.heatmap(cm, annot=True, fmt="d", cmap="Blues",
            xticklabels=label_encoder.classes_,
            yticklabels=label_encoder.classes_)
plt.xlabel("Predicted")
plt.ylabel("True")
plt.title("Confusion Matrix")
plt.show()
```
![[Pasted image 20261024114444.png]]