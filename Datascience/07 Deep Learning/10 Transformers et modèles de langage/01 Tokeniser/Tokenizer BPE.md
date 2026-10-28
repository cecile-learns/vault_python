# Tokenizer BPE

## Syntaxe

```python
from tokenizers import Tokenizer, models, trainers
from tokenizers.normalizers import Lowercase
from tokenizers.pre_tokenizers import Whitespace

tokenizer = Tokenizer(models.BPE())
tokenizer.normalizer = Lowercase()
tokenizer.pre_tokenizer = Whitespace()

trainer = trainers.BpeTrainer(
    vocab_size=5000,
    min_frequency=2,
    special_tokens=["[UNK]", "[PAD]", "[BOS]", "[EOS]"],
)

tokenizer.train_from_iterator(corpus, trainer=trainer)
encodage = tokenizer.encode("Une phrase à tokeniser")
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `vocab_size` | taille maximale du vocabulaire | grand = moins de découpage, modèle plus lourd |
| `min_frequency` | fréquence minimale d'un token | augmente pour éliminer les unités rares |
| `special_tokens` | tokens réservés | cohérents avec l'entraînement du modèle |
| `Lowercase` | normalisation de casse | à éviter si la casse porte de l'information |

## Vigilances & tips

Le tokenizer utilisé en inférence doit être exactement celui utilisé pour préparer l'entraînement.
