# get_openai_callback

## Syntaxe

```python
from langchain.callbacks import get_openai_callback

with get_openai_callback() as callback:
    reponse = llm.invoke(prompt)

print("Tokens :", callback.total_tokens)
print("Coût :", callback.total_cost)
```

## Éléments

| Élément | Rôle |
|---|---|
| `total_tokens` | volume total traité |
| `total_cost` | coût calculé pour les appels compatibles |

## Vigilances & tips

Le suivi dépend de l'intégration et du fournisseur utilisés ; une valeur absente ne signifie pas que l'appel est gratuit.
