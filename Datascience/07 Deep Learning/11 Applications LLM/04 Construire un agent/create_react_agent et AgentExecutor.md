# create_react_agent et AgentExecutor

## Syntaxe

```python
from langchain import hub
from langchain.agents import AgentExecutor, create_react_agent

prompt_agent = hub.pull("hwchase17/react")
agent = create_react_agent(llm, outils, prompt_agent)

executor = AgentExecutor(
    agent=agent,
    tools=outils,
    verbose=True,
    max_iterations=5,
    early_stopping_method="generate",
)

reponse = executor.invoke({"input": question})
```

## Paramètres et hyperparamètres

| Paramètre | Rôle | Réglage utile |
|---|---|---|
| `tools` | actions autorisées | limiter au strict nécessaire |
| `verbose` | affiche les étapes | utile pour déboguer, à réduire ensuite |
| `max_iterations` | limite les boucles | protège contre les agents qui tournent en rond |
| `early_stopping_method` | comportement à la limite | `generate` demande une réponse finale |

## Vigilances & tips

La description des outils influence directement leur sélection. Un agent ne doit pas recevoir un outil plus puissant que nécessaire.
