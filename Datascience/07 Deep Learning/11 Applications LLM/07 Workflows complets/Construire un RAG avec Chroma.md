# Construire un RAG avec Chroma

## Pour faire quoi ?

Retrouver des passages pertinents dans un corpus et les fournir au LLM pour répondre à une question.

## Script réutilisable

```python
from langchain.schema import Document
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_huggingface import HuggingFaceEmbeddings
from langchain_community.vectorstores import Chroma
from langchain.prompts import PromptTemplate
from langchain.schema.output_parser import StrOutputParser
from langchain.schema.runnable import RunnablePassthrough

documents = [Document(page_content=texte) for texte in corpus]

splitter = RecursiveCharacterTextSplitter(
    chunk_size=500,
    chunk_overlap=100,
)
chunks = splitter.split_documents(documents)

embeddings = HuggingFaceEmbeddings(
    model_name="sentence-transformers/all-MiniLM-L6-v2"
)
vectorstore = Chroma.from_documents(chunks, embeddings)
retriever = vectorstore.as_retriever(search_kwargs={"k": 3})

template = PromptTemplate.from_template(
    "Réponds uniquement à partir du contexte.\n\n"
    "Contexte :\n{context}\n\nQuestion : {question}"
)

def formater_documents(docs):
    return "\n\n".join(doc.page_content for doc in docs)

rag_chain = (
    {
        "context": retriever | formater_documents,
        "question": RunnablePassthrough(),
    }
    | template
    | llm
    | StrOutputParser()
)

reponse = rag_chain.invoke(question)
```

## Paramètres et hyperparamètres

| Paramètre | Effet |
|---|---|
| `chunk_size` | quantité de texte dans chaque segment |
| `chunk_overlap` | continuité entre segments |
| modèle d'embeddings | qualité du rapprochement sémantique |
| `k` | nombre de documents envoyés au LLM |
| modèle et température | qualité, coût et stabilité de la réponse |

## Contrôles

- inspecter les documents récupérés ;
- vérifier que la réponse est soutenue par le contexte ;
- tester des questions sans réponse dans le corpus ;
- mesurer séparément retrieval et génération.
