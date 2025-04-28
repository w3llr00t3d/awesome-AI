# <center>Vector Database</center>

## Introduction
A vector database is a database system designed to store and process vector data. Vector data consists of numerical arrays, typically used to represent features or attributes. The goal of a vector database is to efficiently handle and query large-scale vector datasets. With the rise of AI applications in natural language, image recognition, and other unstructured data domains, embedding techniques have become widespread for encoding unstructured data (text, audio, video, etc.) as vectors for machine learning models. Vector databases are now an effective solution for enterprises to deliver and scale these use cases.

## Popular Vector Databases
- [Chroma](#chroma)
- [Faiss](#faiss)
- [Qdrant](#qdrant)
- [Milvus](#milvus)
- [Pinecone](#pinecone)
- [DeepsetAI](#deepsetai)
- [pgvector](#pgvector)
- [Supabase](#supabase)
- [Vespa](#vespa)
- [Weaviate](#weaviate)
- [HNSWLib](#hnswlib)

---

## Chroma
Chroma is a new AI-native open-source embedding database that is lightweight and easy to use. It enables knowledge, facts, and skills to be pluggable, making it easy to build LLM applications. It can run in-memory (with persistence to disk) or as a database server (similar to traditional databases).

[Documentation |](https://docs.trychroma.com)
[GitHub |](https://github.com/chroma-core/chroma)
```py
pip install chromadb

from langchain_openai import OpenAIEmbeddings
from langchain_text_splitters import CharacterTextSplitter
from langchain_community.vectorstores import Chroma
from langchain_community.document_loaders import TextLoader

loader = TextLoader('../../../state_of_the_union.txt')
documents = loader.load()
text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=0)
docs = text_splitter.split_documents(documents)
 
embedding = OpenAIEmbeddings(
    model="text-embedding-3-small",
    openai_api_key=env_vars['OPENAI_API_KEY'],
)
# openai_api_base=env_vars['OPENAI_API_BASE']
db = Chroma.from_documents(docs, embedding)
 
query = "What did the president say about Ketanji Brown Jackson"
docs = db.similarity_search(query)
print(docs[0].page_content)
```

## Faiss
Faiss (Facebook AI Similarity Search) is a library for efficient similarity search and clustering of dense vectors. It contains algorithms for searching in large collections of vectors, including those that may not fit in memory. It also provides code for evaluation and parameter tuning.

[GitHub](https://github.com/facebookresearch/faiss)
```py
pip install faiss-cpu

from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.text_splitter import CharacterTextSplitter
from langchain.vectorstores import FAISS
from langchain.document_loaders import TextLoader
 
loader = TextLoader('../../../state_of_the_union.txt')
documents = loader.load()
text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=0)
docs = text_splitter.split_documents(documents)
 
embedding = OpenAIEmbeddings(
    model="text-embedding-3-small",
    openai_api_key=env_vars['OPENAI_API_KEY'],
)
db = FAISS.from_documents(docs, embedding)
 
query = "What did the president say about Ketanji Brown Jackson"
docs = db.similarity_search(query)
print(docs[0].page_content)
```

## Qdrant
Qdrant is an open-source vector similarity search engine and database for AI applications.

[Documentation |](https://qdrant.tech/documentation/)
[GitHub |](https://github.com/qdrant/qdrant)
```py
pip install qdrant-client

from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.text_splitter import CharacterTextSplitter
from langchain.vectorstores import Qdrant
from langchain.document_loaders import TextLoader
 
loader = TextLoader('../../../state_of_the_union.txt')
documents = loader.load()
text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=0)
docs = text_splitter.split_documents(documents)
 
embedding = OpenAIEmbeddings()

# In-memory
qdrant = Qdrant.from_documents(
    docs, embedding, 
    location=":memory:",  # Local mode with in-memory storage only
    collection_name="my_documents",
)

# Disk storage
qdrant = Qdrant.from_documents(
    docs, embedding, 
    path="/tmp/local_qdrant",
    collection_name="my_documents",
)

query = "What did the president say about Ketanji Brown Jackson"
found_docs = qdrant.similarity_search(query)
print(found_docs[0].page_content) 
```

## Milvus
Milvus is an open-source vector database built for scalable similarity search and AI applications.

[Documentation |](https://milvus.io/docs/overview.md)
[GitHub |](https://github.com/milvus-io/milvus)
```py
pip install pymilvus

from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.text_splitter import CharacterTextSplitter
from langchain.vectorstores import Milvus
from langchain.document_loaders import TextLoader
 
loader = TextLoader('../../../state_of_the_union.txt')
documents = loader.load()
text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=0)
docs = text_splitter.split_documents(documents)
 
embedding = OpenAIEmbeddings()
vector_db = Milvus.from_documents(
    docs,
    embedding,
    connection_args={"host": "127.0.0.1", "port": "19530"},
)
docs = vector_db.similarity_search(query)
docs[0]
```

## Pinecone
Pinecone is a fully managed vector database service for production AI applications.

[Documentation |](https://docs.pinecone.io/docs/overview)
[GitHub |](https://github.com/pinecone-io/pinecone)

## DeepsetAI
DeepsetAI provides vector search and retrieval solutions for enterprise AI.

[Documentation |](https://docs.deepset.ai/docs/overview)
[GitHub |](https://github.com/deepset-ai/haystack)

## pgvector
pgvector is a PostgreSQL extension for vector similarity search.

[Documentation |](https://github.com/pgvector/pgvector)

## Supabase
Supabase is an open-source backend-as-a-service platform that supports vector search with pgvector.

[Documentation |](https://supabase.com/docs/guides/database/extensions/pgvector)

## Vespa
Vespa is an open-source engine for low-latency serving, search, and recommendation using vectors.

[Documentation |](https://docs.vespa.ai/en/)
[GitHub |](https://github.com/vespa-engine/vespa)

## Weaviate
Weaviate is an open-source vector database for storing and searching embeddings.

[Documentation |](https://weaviate.io/developers/weaviate)
[GitHub |](https://github.com/semi-technologies/weaviate)

## HNSWLib
HNSWLib is a fast approximate nearest neighbor search library for dense vectors.

[GitHub |](https://github.com/nmslib/hnswlib)
