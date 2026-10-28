# Tool et outils rencontrés

## Outil personnalisé

```python
from langchain_core.tools import Tool

outil = Tool(
    name="calcul_python",
    func=python_repl.run,
    description="Exécute un calcul Python lorsque la question nécessite un calcul.",
)
```

## Outils du cours

```python
from langchain_community.tools import WikipediaQueryRun
from langchain_community.utilities import WikipediaAPIWrapper, SerpAPIWrapper
from langchain_experimental.utilities import PythonREPL

wikipedia = WikipediaQueryRun(api_wrapper=WikipediaAPIWrapper())
recherche_web = SerpAPIWrapper()
python_repl = PythonREPL()
```

## Paramètres

| Paramètre | Rôle |
|---|---|
| `name` | identifiant lisible par l'agent |
| `func` | fonction réellement exécutée |
| `description` | indique à l'agent quand choisir l'outil |
| wrapper API | configure le service externe utilisé |

## Vigilances & tips

`PythonREPL` exécute réellement du code. Ne jamais lui transmettre une entrée non maîtrisée ou lui donner accès à des secrets et fichiers sensibles.
