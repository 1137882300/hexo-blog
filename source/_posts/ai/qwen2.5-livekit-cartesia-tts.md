---
title: 越狱后的实时聊天小姐姐声音也更甜美了 | QWEN2.5 越狱 | livekit | Cartesia TTS
shortTitle: 越狱后的实时聊天小姐姐声音也更甜美了 | QWEN2.5 越狱 | livekit | Cartesia TTS
categories:
  - [AI]
  - [livekit]
  - [TTS]
tags:
  - qwen
  - livekit
  - tts
description: 越狱后的实时聊天小姐姐声音也更甜美了 | QWEN2.5 越狱 | livekit | Cartesia TTS
date: 2024-10-03
keywords: 'AI,人工智能,越狱,livekit,Cartesia TTS,tts'
cover: https://img.crab6688.cloudns.org/file/4e9f62d1eb418ab891de3-542906f557e3b7d5fc.jpg
abbrlink: 112262
---

## 关于LLM的jailbreak
1. 方法论
- https://www.lesswrong.com/posts/jGuXSZgv6qfdhMCuJ/refusal-in-llms-is-mediated-by-a-single-direction
- https://huggingface.co/blog/mlabonne/abliteration
2. 越狱原模和GGUF（Qwen2.5-7B-Instruct-abliterated-v2）
- https://huggingface.co/huihui-ai/Qwen2.5-7B-Instruct-abliterated-v2
- https://huggingface.co/mradermacher/Qwen2.5-7B-Instruct-abliterated-v2-GGUF
## 配置（main.py - 正常版本，即非越狱版本）
- llm: openai
- stt: openai
- tts: openai
## 配置（main_jb.py: QWEN2.5B越狱版本）
- llm: jail-broken qwen2.5B
- stt: openai (wisper-1)
- tts: cartesia.ai (参考：https://cartesia.ai/pricing）(提供免费额度)
## 视频
- 正常版本：https://youtu.be/7TQXMfCXTjI?si=jM0YoB_VHzaOaDhD
- QWEN2.5B越狱版本: https://youtu.be/V7kSsAoIWxQ?si=9Gc9I7jpdawtusaG

## 代码
```python
import asyncio
from dotenv import load_dotenv
from livekit.agents import AutoSubscribe, JobContext,WorkerOptions, cli, llm
from livekit.agents.voice_assistant import VoiceAssistant
from livekit.plugins import openai, silero,  cartesia


load_dotenv()

async def entry(ctx: JobContext):
    chat_ctx = llm.ChatContext().append(
        role="system",
        text=("你是一个说中文的，并且说话简洁的助手。")
    )

    await ctx.connect(auto_subscribe=AutoSubscribe.AUDIO_ONLY)

    asssitant = VoiceAssistant(
        vad=silero.VAD.load(),
        stt=openai.STT(language="zh"),
        tts=cartesia.TTS(
            model="sonic-multilingual",
            language="zh",
            voice="e90c6678-f0d3-4767-9883-5d0ecf5894a8", #Chinese Female Conversational
            api_key="xxxxx" #replaced with your own api key
            ),
        llm=openai.LLM.with_ollama(base_url="http://localhost:11434/v1", model="aispin/qwen2.5-7b-instruct-abliterated-v2.q4_k_s.gguf:latest"),
        chat_ctx=chat_ctx
    )
    asssitant.start(ctx.room)

    await asyncio.sleep(1)
    await asssitant.say("你好，我是小姐姐，很高兴见到你",allow_interruptions=False)


if __name__ == "__main__":
    cli.run_app(WorkerOptions(entrypoint_fnc=entry))
```