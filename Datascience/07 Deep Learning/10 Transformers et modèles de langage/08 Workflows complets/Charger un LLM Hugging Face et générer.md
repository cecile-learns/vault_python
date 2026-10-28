# Charger un LLM Hugging Face et générer

## Pour faire quoi ?

Charger un modèle causal et produire une continuation de texte.

## Script réutilisable

```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer

nom_modele = "gpt2"
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

tokenizer = AutoTokenizer.from_pretrained(nom_modele)
model = AutoModelForCausalLM.from_pretrained(nom_modele).to(device)
model.eval()

entrees = tokenizer(prompt, return_tensors="pt").to(device)

with torch.no_grad():
    ids_generes = model.generate(
        **entrees,
        max_new_tokens=100,
        do_sample=True,
        temperature=0.8,
        top_p=0.9,
    )

texte = tokenizer.decode(ids_generes[0], skip_special_tokens=True)
print(texte)
```

## Paramètres et hyperparamètres

| Paramètre | Effet |
|---|---|
| `max_new_tokens` | longueur maximale ajoutée |
| `do_sample` | active l'échantillonnage |
| `temperature` | faible = plus conservateur ; élevée = plus varié |
| `top_p` | limite aux tokens couvrant une masse de probabilité cumulée |
| `skip_special_tokens` | masque les tokens techniques au décodage |

## Vigilances & tips

Les réglages de génération modifient la sortie, pas les poids du modèle.
