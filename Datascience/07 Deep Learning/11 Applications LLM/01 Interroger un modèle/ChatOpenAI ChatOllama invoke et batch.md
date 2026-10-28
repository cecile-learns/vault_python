# ChatOpenAI, ChatOllama, invoke et batch

## Syntaxe

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    api_key=api_key,
    model="gpt-4o-mini",
    temperature=0,
)

reponse = llm.invoke("Votre prompt")
reponses = llm.batch(["Prompt 1", "Prompt 2"])
```

Avec un modèle local rencontré dans le cours :

```python
from langchain_ollama import ChatOllama

llm_local = ChatOllama(model="llama3.1", temperature=0)
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `model` | modèle appelé | coût, vitesse et capacité différents |
| `temperature` | variabilité | `0` pour une extraction plus stable |
| `api_key` | authentification | ne pas l'écrire en clair dans le notebook |
| `reasoning_effort` | raisonnement des modèles compatibles | le cours utilise `none` avec un modèle Groq raisonneur |
| `batch` | traite plusieurs prompts | utile pour un corpus, mais surveiller coûts et limites API |

## Vigilances & tips

Une réponse de modèle reste à valider. La stabilité d'une sortie structurée dépend du modèle, du prompt et des paramètres.
