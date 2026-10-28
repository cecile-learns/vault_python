# PromptTemplate et StrOutputParser

## Syntaxe

```python
from langchain.prompts import PromptTemplate
from langchain.schema.output_parser import StrOutputParser

template = PromptTemplate(
    input_variables=["topic", "level", "nb_phrases"],
    template="Explique {topic} à un niveau {level}, en {nb_phrases} phrases.",
)

prompt = template.format(
    topic="le LLMOps",
    level="intermédiaire",
    nb_phrases="2",
)

chaine = template | llm | StrOutputParser()
```

## Paramètres

| Paramètre | Rôle |
|---|---|
| `input_variables` | noms des valeurs injectées |
| `template` | texte et emplacements `{variable}` |
| `StrOutputParser` | retourne directement une chaîne |

## Vigilances & tips

Les noms passés à `format` ou `invoke` doivent correspondre exactement aux variables du template.
