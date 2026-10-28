# PyTorch Lightning

## Pattern

```python
from pytorch_lightning import LightningModule, Trainer
import torch
import torch.nn as nn

class ModeleLightning(LightningModule):
    def __init__(self, nombre_features, nombre_classes, lr=1e-3):
        super().__init__()
        self.save_hyperparameters()
        self.model = nn.Sequential(
            nn.Linear(nombre_features, 100),
            nn.ReLU(),
            nn.Linear(100, nombre_classes),
        )
        self.loss_fn = nn.CrossEntropyLoss()

    def forward(self, x):
        return self.model(x)

    def training_step(self, batch, batch_idx):
        x, y = batch
        loss = self.loss_fn(self(x), y)
        self.log("train_loss", loss)
        return loss

    def configure_optimizers(self):
        return torch.optim.Adam(self.parameters(), lr=self.hparams.lr)

model = ModeleLightning(nombre_features, nombre_classes, lr=1e-3)
trainer = Trainer(max_epochs=20)
trainer.fit(model, train_loader)
```

## Paramètres et hyperparamètres

| Paramètre | Rôle |
|---|---|
| `lr` | learning rate conservé comme hyperparamètre |
| `max_epochs` | durée maximale |
| `training_step` | traitement d'un batch train |
| `configure_optimizers` | optimiseur et éventuel scheduler |
