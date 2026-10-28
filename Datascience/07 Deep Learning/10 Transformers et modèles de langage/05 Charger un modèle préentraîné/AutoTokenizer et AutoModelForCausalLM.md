# AutoTokenizer et AutoModelForCausalLM

## Syntaxe

```python
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer

nom_modele = "gpt2"
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

tokenizer = AutoTokenizer.from_pretrained(nom_modele)
model = AutoModelForCausalLM.from_pretrained(nom_modele).to(device)
model.eval()

entrees = tokenizer(
    prompt,
    return_tensors="pt",
    truncation=True,
    max_length=512,
).to(device)
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| nom du modèle | checkpoint chargé | compatible avec la tâche et les ressources |
| `return_tensors="pt"` | retourne des tenseurs PyTorch | requis avec ce modèle |
| `truncation` | coupe les séquences trop longues | éviter les erreurs de longueur |
| `max_length` | longueur maximale encodée | dépend du modèle et de la tâche |

## Vigilances & tips

Le tokenizer et le modèle doivent provenir du même checkpoint ou être explicitement compatibles.
