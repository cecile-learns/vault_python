# Entraîner un tokenizer BPE

## Pour faire quoi ?

Apprendre un vocabulaire de sous-mots directement sur un corpus.

## Script réutilisable

```python
from tokenizers import Tokenizer, models, trainers
from tokenizers.normalizers import Lowercase
from tokenizers.pre_tokenizers import Whitespace

tokenizer = Tokenizer(models.BPE(unk_token="[UNK]"))
tokenizer.normalizer = Lowercase()
tokenizer.pre_tokenizer = Whitespace()

trainer = trainers.BpeTrainer(
    vocab_size=5000,
    min_frequency=2,
    special_tokens=["[UNK]", "[PAD]", "[BOS]", "[EOS]"],
)

tokenizer.train_from_iterator(corpus, trainer=trainer)

test = tokenizer.encode("Phrase de contrôle")
print(test.tokens)
print(test.ids)
```

## À adapter

- corpus ;
- taille du vocabulaire ;
- fréquence minimale ;
- normalisation et pré-tokenisation ;
- tokens spéciaux attendus par le modèle.
