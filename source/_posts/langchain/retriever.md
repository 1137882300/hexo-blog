---
title: retriever
shortTitle:
categories:
  - [ AI ]
  - [ LLM ]
  - [ LangChain ]
tags:
  - AI
  - LLM
  - 大语言模型
  - LangChain
  - RAG
description:
date: 2024-06-04
keywords: 'AI,prompt,LangChain'
cover: https://img2.funning.top/file/013f0d9a3f50c73d4b310.png
abbrlink: 112257
---

# 初始化AI实例

```python
from dotenv import load_dotenv
import os
from langchain_openai import ChatOpenAI

# 加载当前目录下的.env文件
# load_dotenv()
# load_dotenv(override=True) 会重新读取.env
load_dotenv(override=True)

# 现在可以像访问普通环境变量一样访问.env文件中的变量了
api_key = os.getenv('OPENAI_API_KEY')
base_url = os.getenv('OPENAI_API_BASE')

print(api_key)
print(base_url)

chat = ChatOpenAI(
    openai_api_base=base_url,
    openai_api_key=api_key,
    model_name="moonshot-v1-8k",
    temperature=0.7,
)
```

# 初始化向量化的实例

```python
from langchain.embeddings import SentenceTransformerEmbeddings

embeddings_model = SentenceTransformerEmbeddings(model_name="all-MiniLM-L6-v2")
```

# 加载文件

```python
from langchain_community.document_loaders import TextLoader

file_path = '../file/最后一个问题.txt'
loader = TextLoader(file_path, encoding='UTF-8')
data = loader.load()
print(data)

# from langchain_community.document_loaders.csv_loader import CSVLoader
# loader = CSVLoader(file_path='./mlb_teams_2012.csv')

with open(file_path, encoding='UTF-8') as f:
    # 读到的文件内容存在变量
    last_question = f.read()
```

# 分割器定义

```python
from langchain_text_splitters import CharacterTextSplitter

# 定义分割器
text_splitter = CharacterTextSplitter(
    separator="\n\n",
    chunk_size=450,
    chunk_overlap=100,
    length_function=len,
    is_separator_regex=False,
)
```

# 分割文件内容

```python
texts = text_splitter.create_documents([last_question])
print(texts[0])
print(len(texts))
```

# 向量化demo

```python
# 向量化demo
embeddings = embeddings_model.embed_documents(
    [
        "Hi there!",
        "Oh, hello!",
        "What's your name?",
        "My friends call me World",
        "Hello World!"
    ]
)
len(embeddings), len(embeddings[0])
```

# 向量化1

```python
# 类型转换，转成embed_documents接收的参数类型
texts_new = []
for text in texts:
    texts_new.append(text.page_content)

print(type(texts_new))
print(texts_new)

# 向量化方式1:embed_documents
# 向量化texts
embeddings_new = embeddings_model.embed_documents(
    texts_new
)
len(embeddings_new), len(embeddings_new[0])
```

# 向量化2

```python
# 向量化
# 向量化方式2:embed_query
embedded_query = embeddings_model.embed_query("艾文是谁?")
# 前五个
embedded_query[:5]
```

# 本地向量数据库

```python
# pip install chromadb

from langchain_community.vectorstores import Chroma

# 保存到本地向量数据库里
db = Chroma.from_documents(texts, embeddings_model)
query = "赛琳的父母"
# 相关性的查询
docs = db.similarity_search(query)
print(docs[0].page_content)
```

# 相似性查询

```python
# Vector store-backed retriever
retriever = db.as_retriever()
docs = retriever.get_relevant_documents("于是就有了光")
print(docs[0].page_content)

retriever = db.as_retriever(search_type="mmr")
docs = retriever.get_relevant_documents("于是就有了光")
print(docs[0].page_content)
```

# 完整例子

```python
from langchain_community.document_loaders import WebBaseLoader
from langchain_text_splitters import RecursiveCharacterTextSplitter

loader = WebBaseLoader(
    "https://blog.google/technology/ai/google-gemini-next-generation-model-february-2024/?utm_source=yt&utm_medium=social&utm_campaign=gemini24#build-experiment")
data2 = loader.load()

text_splitter2 = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=0)
splits = text_splitter2.split_documents(data2)
print(len(splits))

# 向量化，并存到本地向量数据库里
vectordb = Chroma.from_documents(documents=splits, embedding=embeddings_model)

from langchain.retrievers.multi_query import MultiQueryRetriever
from langchain_openai import ChatOpenAI

# 相似性查询，单纯的去计算两个向量的距离，在有些时候在语意上有些不正确，所以MultiQueryRetriever就是解决这个问题的
# MultiQueryRetriever 更高级的，它可以使用大模型从不同角度为给用户的输入查询生成多个不同纬度的查询，对于每个查询都会检索一组相关性文档，
# 再把这些文档做个并集，使得结果更加准确

question = "does gemini can perform understanding and reasoning tasks for different modalities"
# llm = ChatOpenAI(temperature=0)
retriever_from_llm = MultiQueryRetriever.from_llm(
    retriever=vectordb.as_retriever(), llm=chat
)

import logging

# 打印内部生成的日志信息
logging.basicConfig()
logging.getLogger("langchain.retrievers.multi_query").setLevel(logging.INFO)

unique_docs = retriever_from_llm.get_relevant_documents(query=question)
len(unique_docs)

print(unique_docs[1])
```

# 总结

```python
# 向量化
from langchain.embeddings import SentenceTransformerEmbeddings
from langchain_community.vectorstores import Chroma

embeddings_model = SentenceTransformerEmbeddings(model_name="all-MiniLM-L6-v2")
# 向量化方式 
texts_new = []
# 方式1a
embeddings_new = embeddings_model.embed_documents(texts_new)
# 方式2
embedded_query = embeddings_model.embed_query("艾文是谁?")
# 方式3
db = Chroma.from_documents(texts_new, embeddings_model)
```

```python
# 相似性查询
from langchain_community.vectorstores import Chroma

texts = []
# 向量化
db2 = Chroma.from_documents(texts, embeddings_model)
query = "赛琳的父母"
# 相关性的查询
# 方式1
docs1 = db.similarity_search(query)

# Vector store-backed retriever
retriever = db.as_retriever(search_type="mmr")
# 方式2: 更高级，不限于用在向量数据里的查询上
docs2 = retriever.get_relevant_documents("于是就有了光")
```