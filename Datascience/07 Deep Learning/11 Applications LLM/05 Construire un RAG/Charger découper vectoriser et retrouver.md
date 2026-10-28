# Charger, découper, vectoriser et retrouver

## Syntaxe

```python
from langchain.schema import Document
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_huggingface import HuggingFaceEmbeddings
from langchain_community.vectorstores import Chroma

documents = [Document(page_content=texte) for texte in corpus]

splitter = RecursiveCharacterTextSplitter(
    chunk_size=100,
    chunk_overlap=40,
    separators=["\n\n", "\n", ".", " "],
)
chunks = splitter.split_documents(documents)

embeddings = HuggingFaceEmbeddings(
    model_name="sentence-transformers/all-MiniLM-L6-v2"
)
vectorstore = Chroma.from_documents(chunks, embeddings)
retriever = vectorstore.as_retriever(search_kwargs={"k": 3})

documents_proches = retriever.invoke(question)
```

## Paramètres et hyperparamètres

| Paramètre | Effet |
|---|---|
| `chunk_size` | quantité de contexte par segment |
| `chunk_overlap` | continuité entre segments voisins |
| `separators` | endroits privilégiés pour couper |
| modèle d'embeddings | représentation sémantique et langue |
| `k` | nombre de segments remis au LLM |

## Vigilances & tips

Un `k` ou des chunks plus grands augmentent le contexte mais aussi le bruit et le coût. Contrôler les documents retrouvés avant d'évaluer la génération.
