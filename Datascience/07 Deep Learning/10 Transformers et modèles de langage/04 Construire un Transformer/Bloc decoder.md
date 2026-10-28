# Bloc decoder

## Éléments rencontrés

```python
self.embedding = nn.Embedding(vocab_size, d_model)
self.dec_layers = nn.ModuleList([
    DecoderLayer(d_model, num_heads, dff, rate)
    for _ in range(num_layers)
])
self.final_layer = nn.Linear(d_model, vocab_size)
```

Dans une couche :

```python
self.layernorm1 = nn.LayerNorm(d_model)
self.layernorm2 = nn.LayerNorm(d_model)
self.dropout1 = nn.Dropout(rate)
self.dropout2 = nn.Dropout(rate)
self.ffn = nn.Sequential(
    nn.Linear(d_model, dff),
    nn.ReLU(),
    nn.Linear(dff, d_model),
)
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Effet du réglage |
|---|---|---|
| `d_model` | dimension interne | capacité et coût mémoire |
| `num_heads` | têtes d'attention | doit être compatible avec `d_model` |
| `dff` | largeur du réseau feed-forward | capacité par couche |
| `num_layers` | profondeur | augmente la capacité et le coût |
| `rate` | dropout | régularisation |
| `vocab_size` | taille de la sortie finale | correspond au tokenizer |
