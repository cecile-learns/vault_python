# Construire un agent ReAct

## Pour faire quoi ?

Autoriser un LLM à choisir parmi quelques outils, observer leur résultat puis construire une réponse.

## Script réutilisable

```python
from langchain import hub
from langchain.agents import AgentExecutor, create_react_agent

outils = [outil_wikipedia, outil_python]
prompt_agent = hub.pull("hwchase17/react")

agent = create_react_agent(llm, outils, prompt_agent)
executor = AgentExecutor(
    agent=agent,
    tools=outils,
    verbose=True,
    max_iterations=5,
    early_stopping_method="generate",
)

resultat = executor.invoke({"input": question})
print(resultat["output"])
```

## À adapter

- modèle ;
- outils et leurs descriptions ;
- limite d'itérations ;
- prompt d'agent ;
- autorisations accordées aux outils.

## Vigilances & tips

Tester chaque outil séparément avant de le confier à l'agent. Ne pas intégrer `PythonREPL` dans un environnement contenant des données sensibles.
