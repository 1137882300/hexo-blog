---
title: 酷炫 Streamlit 多页面应用开发 - 30分钟完成 LangChain 应用开发与部署
shortTitle:
categories:
  - [ AI ]
  - [ LangChain ]
tags:
  - AI
  - LangChain
  - streamlit
description:
date: 2024-06-19
keywords: 'AI,prompt,LangChain,streamlit'
cover: https://img.crab888.cloudns.org/file/613b0c95ca0e135e4f1f3.png
abbrlink: 112259
---

# 安装所需依赖包

```shell
pip install -q -U langchain openai tiktoken streamlit
```

# 体验地址

[点击体验](https://awesome-app-langchain-robus.streamlit.app/OpenAI)

# 免费部署地址

[部署入口](https://share.streamlit.io/)

# Github 仓库

[Github地址](https://github.com/1137882300/awesome-streamlit-langchain)

# 部分截图

![](https://img.crab888.cloudns.org/file/d70d46cdfe38b9a87ce1b.png)
![](https://img.crab888.cloudns.org/file/dd16776b8ac48b3959403.png)

# 主入口代码

```python
import streamlit as st
from langchain_community.chat_models import ChatOpenAI
from langchain.schema import (
    AIMessage,
    HumanMessage,
)

# Initialize the ChatOpenAI object
chat = None

if "OPENAI_API_BASE" not in st.session_state:
    st.session_state["OPENAI_API_BASE"] = ""

if "OPENAI_API_KEY" not in st.session_state:
    st.session_state["OPENAI_API_KEY"] = ""
elif st.session_state["OPENAI_API_KEY"] != "":
    chat = ChatOpenAI(openai_api_key=st.session_state["OPENAI_API_KEY"],
                      openai_api_base=st.session_state["OPENAI_API_BASE"],
                      model_name="moonshot-v1-8k")

if "PINECONE_API_KEY" not in st.session_state:
    st.session_state["PINECONE_API_KEY"] = ""

if "PINECONE_ENVIRONMENT" not in st.session_state:
    st.session_state["PINECONE_ENVIRONMENT"] = ""

st.set_page_config(page_title="Welcome to ASL", layout="wide")

st.title("🤠 Welcome to ASL")

if "messages" not in st.session_state:
    st.session_state["messages"] = []

if chat:
    with st.container():
        st.header("Chat with GPT")

        for message in st.session_state["messages"]:
            if isinstance(message, HumanMessage):
                with st.chat_message("user"):
                    st.markdown(message.content)
            elif isinstance(message, AIMessage):
                with st.chat_message("assistant"):
                    st.markdown(message.content)
        prompt = st.chat_input("Type something...")
        if prompt:
            st.session_state["messages"].append(HumanMessage(content=prompt))
            with st.chat_message("user"):
                st.markdown(prompt)
            ai_message = chat([HumanMessage(content=prompt)])
            st.session_state["messages"].append(ai_message)
            with st.chat_message("assistant"):
                st.markdown(ai_message.content)
else:
    with st.container():
        st.warning("Please set your OpenAI API key in the settings page.")
```

# Open AI 页面设置代码

```python
import streamlit as st

if "OPENAI_API_KEY" not in st.session_state:
    st.session_state["OPENAI_API_KEY"] = ""

if "OPENAI_API_BASE" not in st.session_state:
    st.session_state["OPENAI_API_BASE"] = ""

st.set_page_config(page_title="OpenAI Settings", layout="wide")

st.title("OpenAI Settings")

openai_api_key = st.text_input("API Key", value=st.session_state["OPENAI_API_KEY"], max_chars=None, key=None,
                               type='password')
openai_api_base = st.text_input("BASE URL", value=st.session_state["OPENAI_API_BASE"], max_chars=None, key=None,
                                type='default')

saved = st.button("Save")

if saved:
    st.session_state["OPENAI_API_KEY"] = openai_api_key
    st.session_state["OPENAI_API_BASE"] = openai_api_base
```

# 最后

> 遇到什么问题，可以给我留言吧，看到会及时回复哦！😊

