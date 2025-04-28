# <center> Chroma </center>

<!-- TOC -->
- [<center> Chroma </center>](#center-chroma--center)
    - [Vector Database Chroma](#vector-database-chroma)
    - [Basic Usage](#basic-usage)
    - [Chroma Embedding](#chroma-embedding)
    - [Chroma Docker](#chroma-docker)
    - [Docker Authentication](#docker-authentication)
    - [Modify Docker Configuration](#modify-docker-configuration)
    - [Usage in LangChain](#usage-in-langchain)
    - [Add Text](#add-text)
    - [Update and Delete Data](#update-and-delete-data)
    - [Similarity Score Retrieval](#similarity-score-retrieval)
    - [Metadata Filtering](#metadata-filtering)
    - [Direct Storage and Query](#direct-storage-and-query)
    - [httpClient](#httpclient)
<!-- /TOC -->


## Vector Database Chroma
[GitHub |](https://github.com/chroma-core/chroma)
[Documentation |](https://docs.trychroma.com/)
[Chroma Usage Guide |](https://zhuanlan.zhihu.com/p/632629938)
[Reference |](https://zhuanlan.zhihu.com/p/628187163)
[Filtering Guide |](https://zhuanlan.zhihu.com/p/640424318)

Chroma is a new AI-native open-source embedding database that is lightweight and easy to use. It enables knowledge, facts, and skills to be pluggable, making it easy to build LLM applications. It can run in-memory (with persistence to disk) or as a database server (similar to traditional databases).

```py
pip install chromadb   # 0.4.3  0.4.24  0.5.0  0.5.8  0.5.18
# pip install chromadb -i https://pypi.tuna.tsinghua.edu.cn/simple  # Switch source for China
# pip install --upgrade chromadb -i https://pypi.tuna.tsinghua.edu.cn/simple # Upgrade
# Recommended: Python 3.10 environment
```

## Basic Usage
[Reference](https://docs.trychroma.com/api-reference)
```py
import chromadb

# Create client
# client = chromadb.Client()  # In-memory mode
client = chromadb.PersistentClient(path="./chromac")  # Data saved to disk
# chroma_client = chromadb.HttpClient(host="localhost", port=8000)  # Docker client mode

# List collections
client.list_collections()
# Create new collection
collection = client.create_collection("testname")
# Get collection
collection = client.get_collection("testname")
# Create or get collection
collection = client.get_or_create_collection("testname")
# Delete collection
client.delete_collection("testname")

# Create or get collection with embedding function
collection = client.get_or_create_collection(name="my_collection2")
# collection = client.create_collection(name="my_collection2")
# collection = client.create_collection(name="my_collection", embedding_function=emb_fn)
# collection = client.get_collection(name="my_collection", embedding_function=emb_fn)
# When creating a Chroma collection, you can specify a name and an optional embedding function. If an embedding function is provided, it must be provided every time you access the collection.

# Peek at the latest 5 entries in the collection
collection.peek()

# Add data
collection.add(
    documents=["On February 2, 2022, the US Department of Defense announced: more troops will be sent to Europe in response to tensions on the Russia-Ukraine border.", "On February 17, the Ukrainian military claimed: eastern militias launched artillery attacks on government-controlled areas, while the eastern militias accused the government of using heavy weapons first. Tensions in eastern Ukraine continue to escalate."],
    metadatas=[{"source": "my_source"}, {"source": "my_source"}],
    ids=["id1", "id2"]
)

# Update data
# Update the embedding, metadata, or document for the provided id.
def update(ids: OneOrMany[ID],
           embeddings: Optional[OneOrMany[Embedding]] = None,
           metadatas: Optional[OneOrMany[Metadata]] = None,
           documents: Optional[OneOrMany[Document]] = None) -> None

# Upsert: update if exists, create if not
# Update or create the embedding, metadata, or document for the provided id.
def upsert(ids: OneOrMany[ID],
        embeddings: Optional[OneOrMany[Embedding]] = None,
        metadatas: Optional[OneOrMany[Metadata]] = None,
        documents: Optional[OneOrMany[Document]] = None) -> None

# Delete data
# Delete embeddings by ID and/or where filter
def delete(ids: Optional[IDs] = None,
           where: Optional[Where] = None,
           where_document: Optional[WhereDocument] = None) -> None
# collection.delete(ids=["3", "4", "5"])

# Query data
results = collection.query(
    query_embeddings=[[11.1, 12.1, 13.1],[1.1, 2.3, 3.2], ...],
    n_results=10,
    where={"metadata_field": "is_equal_to_this"},
    where_document={"$contains":"search_string"}
)
# or
results = collection.query(
    query_texts=["When did the Russia-Ukraine war start?"],
    n_results=1
)
print(results)

# Example output:
# {'ids': [['ab51abbe-6d3f-4c0e-a2cc-b245a3811ae9']], 'embeddings': None, 'documents': [['...']], 'metadatas': [[None]], 'distances': [[0.9033191204071045]]}
```

## Chroma Embedding
Chroma provides lightweight wrappers for popular embedding providers, making it easy to use them in your application. You can set an embedding function when creating a Chroma collection, or call them directly.
```py
from chromadb.utils import embedding_functions

class MyEmbeddingFunction(EmbeddingFunction):
    def __call__(self, texts: Documents) -> Embeddings:
        # Embed the documents here
        return embeddings

# OpenAI Embedding API
from langchain_openai import OpenAIEmbeddings
from langchain_text_splitters import CharacterTextSplitter
from langchain_community.vectorstores import Chroma
from langchain_community.document_loaders import TextLoader
import os

os.environ["OPENAI_API_KEY"] = 'sk-xxxxxx'
loader = TextLoader('./russia.txt', encoding='gbk')  # For Chinese, use encoding='gbk'
documents = loader.load()
text_splitter = CharacterTextSplitter(chunk_size=400, chunk_overlap=0)
docs = text_splitter.split_documents(documents)
embedding = OpenAIEmbeddings(
    model="text-embedding-3-small",
    openai_api_key=os.environ["OPENAI_API_KEY"],
)
db = Chroma.from_documents(docs, embedding)
query = "What did the president say about Ketanji Brown Jackson?"
docs = db.similarity_search(query)
print(docs[0].page_content)
```

## Chroma Docker
Chroma can be run as a Docker service.
```sh
docker pull lemooljiang/chroma-server:latest
# Start the service
docker run -p 8000:8000 chromadb/chroma
docker run -d -p 8000:8000 --name chromadb lemooljiang/chroma-server
```

## Docker Authentication
To enable authentication:
```sh
export CHROMA_SERVER_AUTHN_PROVIDER="chromadb.auth.basic_authn.BasicAuthenticationServerProvider"
```

## Modify Docker Configuration
To build and run with Docker Compose:
```sh
docker compose up -d --build
```

## Usage in LangChain
```py
import chromadb
from chromadb.config import Settings

# Client-side
client = chromadb.HttpClient(
    host='47.113.xxx.xxx', port=8000,  # host='localhost' for local
    settings=Settings(chroma_client_auth_provider="chromadb.auth.basic_authn.BasicAuthClientProvider",
                      chroma_client_auth_credentials="admin:admin")
)

from langchain_openai import OpenAIEmbeddings
from langchain_text_splitters import CharacterTextSplitter
from langchain_community.vectorstores import Chroma
from langchain_community.document_loaders import TextLoader
import os

os.environ["OPENAI_API_KEY"] = 'sk-xxxxxx'
loader = TextLoader('./russia.txt', encoding='gbk')
documents = loader.load()
text_splitter = CharacterTextSplitter(chunk_size=400, chunk_overlap=0)
docs = text_splitter.split_documents(documents)
embedding = OpenAIEmbeddings(
    model="text-embedding-3-small",
    openai_api_key=os.environ["OPENAI_API_KEY"],
)
vectordb = Chroma(collection_name="testNN", embedding_function=embedding, client=client)
print(33, vectordb)
```

## Add Text
```py
collection.add(
    documents=["Sample text 1", "Sample text 2"],
    metadatas=[{"source": "source1"}, {"source": "source2"}],
    ids=["id1", "id2"]
)
```

## Update and Delete Data
```py
collection.update(ids=[...], documents=[...])
collection.delete(ids=[...])
```

## Similarity Score Retrieval
```py
results = db.similarity_search_with_score("foo", filter=dict(page=1))
for doc, score in results:
    print(f"Content: {doc.page_content}, Metadata: {doc.metadata}, Score: {score}")
```

## Metadata Filtering
```py
# Filter collection for updated source
example_db.get(where={"source": "some_other_source"})
# Output: {'ids': [], 'embeddings': None, 'metadatas': [], 'documents': []}
```

## Direct Storage and Query
Not recommended for Chinese—default embedding functions do not handle Chinese well. Use alternatives for better results.
```py
# Store to database
import chromadb
from chromadb.config import Settings
from langchain_text_splitters import CharacterTextSplitter
from langchain_community.document_loaders import TextLoader
import uuid

client = chromadb.Client(Settings(chroma_db_impl="duckdb+parquet", persist_directory="./chromadb"))
collection = client.get_or_create_collection(name="my_collection8")
loader = TextLoader('./russia.txt', encoding='gbk')
documents = loader.load()
text_splitter = CharacterTextSplitter(chunk_size=400, chunk_overlap=0)
docs = text_splitter.split_documents(documents)
Adocs = [doc.page_content for doc in docs]
Ids = [str(uuid.uuid4()) for _ in docs]
collection.add(documents=Adocs, ids=Ids)
print("saveDb ok", collection)

# Query
collection = client.get_collection(name="my_collection8")
results = collection.query(
    query_texts=["When did the Russia-Ukraine war start?"],
    n_results=1
)
print(results)
```

## httpClient
If running the server via Docker, connect directly:
```py
import chromadb
from langchain_community.vectorstores import Chroma
from langchain_openai import OpenAIEmbeddings

collection = 'testNN'
embedding = OpenAIEmbeddings(
    model="text-embedding-3-small",
    openai_api_key=env_vars['OPENAI_API_KEY']
)
httpClient = chromadb.HttpClient(host='139.111.1xx.23', port=8000)
vectordb = Chroma(collection_name=collection, embedding_function=embedding, client=httpClient)
print(vectordb)