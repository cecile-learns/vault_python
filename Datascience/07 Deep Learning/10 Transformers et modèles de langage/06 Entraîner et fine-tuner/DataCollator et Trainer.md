# DataCollator et Trainer

## Pattern

```python
from transformers import DataCollatorForLanguageModeling, Trainer

data_collator = DataCollatorForLanguageModeling(
    tokenizer=tokenizer,
    mlm=False,
)

trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=dataset_train,
    eval_dataset=dataset_validation,
    data_collator=data_collator,
)

trainer.train()
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `mlm=False` | entraînement causal et non masked LM | cohérent avec `AutoModelForCausalLM` |
| train/eval dataset | données d'apprentissage et validation | ne pas utiliser le test pour régler |
| batch size | lots d'entraînement | dépend fortement de la mémoire disponible |
| learning rate | vitesse d'adaptation | souvent plus faible en fine-tuning |
| epochs | passages sur les données | surveiller la validation |
| longueur de séquence | tokens par exemple | principal facteur de mémoire |
