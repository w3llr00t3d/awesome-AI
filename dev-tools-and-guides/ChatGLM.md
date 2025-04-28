# <center>ChatGLM</center>

## Download & Resources
[github |](https://github.com/THUDM/ChatGLM-6B)
[github2 |](https://github.com/THUDM/ChatGLM2-6B)
[Official Website |](https://chatglm.cn/)
[Zhipu AI |](https://open.bigmodel.cn)
[Documentation |](https://open.bigmodel.cn/doc/api)
[Course ｜](https://huggingface.co/learn/nlp-course/zh-CN/chapter0/1?fw=pt)
[Local Knowledge Base ｜](https://github.com/imClumsyPanda/langchain-ChatGLM)
[Local Knowledge Base 2 ｜](https://github.com/thomas-yanxin/LangChain-ChatGLM-Webui)
[DB-GPT |](https://github.com/csunny/DB-GPT)
[ChatLongDoc |](https://github.com/webpilot-ai/ChatLongDoc)
[Build a Personal Knowledge Base with ChatGLM-6B |](https://baijiahao.baidu.com/s?id=1765950735847976093&wfr=spider&for=pc)
[Installation |](https://baijiahao.baidu.com/s?id=1769236478823582890&wfr=spider&for=pc)
[ChatGLM-6B Deployment and P-Tuning Fine-tuning Practice](https://baijiahao.baidu.com/s?id=1765123631287305087)
[Video Link](https://www.bilibili.com/video/BV1414y1m7mE)
[Parse Novels with Langchain-ChatGLM |](https://mp.weixin.qq.com/s/6TckgOO3ZKS9lDhHOq5h0A)
[Deep Learning of Langchain-ChatGLM Middleware |](https://mp.weixin.qq.com/s/m6JZvUPU2lzRSPlbKtXABA)
[Integrate into LangChain |](https://juejin.cn/post/7226157821708681277)
[Notes on QA with LLM |](https://zhuanlan.zhihu.com/p/627439522)

ChatGLM-6B is a bilingual (Chinese-English) conversational language model with 6.2 billion parameters. It is pre-trained on a dataset of over 100 billion words, covering various types and domains of text, including news, encyclopedias, social media, novels, movie scripts, etc., with a special focus on dialogue data such as TV scripts, chat logs, and Q&A platforms. This rich and diverse dataset enables ChatGLM-6B to learn comprehensive and in-depth language knowledge and better adapt to different styles and topics of conversation.

With these optimizations, ChatGLM-6B can be deployed locally on consumer-grade graphics cards and enables real-time conversational interaction. According to Tsinghua KEG Lab and Zhipu AI, the model can run with as little as 6GB of VRAM at INT4 quantization and achieve inference speeds of 10 sentences per second (20 words per sentence) on an RTX 3090.

## Environment
```
//python 3.10.6  pip 23.1.2
virtualenv glm_env
source glm_env/bin/activate
source glm_env/Scripts/activate   //windows
```

## Installation
```py
pip install zhipuai   // 1.0.7  2.0.1
// pip install zhipuai  -i https://pypi.tuna.tsinghua.edu.cn/simple
//Upgrade
pip install --upgrade zhipuai 

import zhipuai
zhipuai.model_api.invoke(
    model="specific model code",
    ... # specific model parameters
)

import zhipuai

zhipuai.api_key = "your api key"
response = zhipuai.model_api.invoke(
    model="chatglm_lite",
    prompt=[
        {"role": "user", "content": "Hello"},
        {"role": "assistant", "content": "I am an AI assistant"},
        {"role": "user", "content": "What is your name"},
        {"role": "assistant", "content": "My name is ChatGLM"},
        {"role": "user", "content": "What can you do"},
    ]
)

def invoke_example():
    response = zhipuai.model_api.invoke(
        model="chatglm_lite",
        prompt=[{"role": "user", "content": "Artificial Intelligence"}],
        top_p=0.7,
        temperature=0.9,
    )
    print(response)
```

## Streaming
[Streaming ChatGPT Interface |](https://blog.csdn.net/time_forgotten/article/details/130437413)
[github |](https://github.com/wemio/chatGPTFlaskWebAPI)
```py
# SSE response is a string stream format, first look at the specific response example
def sse_invoke_example():
    response = zhipuai.model_api.sse_invoke(
        model="chatglm_lite",
        prompt=[{"role": "user", "content": "Artificial Intelligence"}],
        top_p=0.7,
        temperature=0.9,
    )

    for event in response.events():
        if event.event == "add":
            print(event.data)
        elif event.event == "error" or event.event == "interrupted":
            print(event.data)
        elif event.event == "finish":
            print(event.data)
            print(event.meta)
        else:
            print(event.data)
///////
id: "fb981fde-0080-4933-b87b-4a29eaba8d17"
event: "add"
data: "As a"
 
id: "fb981fde-0080-4933-b87b-4a29eaba8d17"
event: "add"
data: "large language model"
 
id: "fb981fde-0080-4933-b87b-4a29eaba8d17"
event: "add"
data: "I can"
 
... ...
 
Id: "fb981fde-0080-4933-b87b-4a29eaba8d17"
event: "finish"
meta: {"request_id":"123445676789","task_id":"75931252186628","task_status":"SUCCESS","usage":{"prompt_tokens":215,"completion_tokens":302,"total_tokens":517}}

eg:
def streaming(query, temperature):
    response = zhipuai.model_api.sse_invoke(
        model = "chatglm_lite",
        prompt = query,
        top_p = 0.7,
        temperature = temperature,
    )
    for event in response.events():
        if event.event == "add":
            yield event.data
        elif event.event == "error" or event.event == "interrupted":
            print(444, event, file=sys.stderr)
            return "error", 500
        elif event.event == "finish":
            return
        else:
            return
```

## Custom LLM Wrapper
```py
pip install langchain -i https://pypi.tuna.tsinghua.edu.cn/simple  (switch to domestic source)

from typing import Dict, List, Optional, Tuple, Union
from langchain.llms.base import LLM
import zhipuai

# os.environ["zhipuai.api_key"] = "d86xxxx"
zhipuai.api_key = "d862xxxxx"

class ChatLLM(LLM):
    def __init__(self):
        super().__init__()

    @property
    def _llm_type(self) -> str:
        return "ChatLLM"

    def _call(self, prompt: str, stop: Optional[List[str]] = None) -> str:
        if stop is not None:
            raise ValueError("stop kwargs are not permitted.")
        response = zhipuai.model_api.invoke(
            model="chatglm_lite",
            prompt=[{"role": "user", "content": prompt}],
            top_p=0.7,
            temperature=0.9,
        )
        print(45, response['data']['choices'][0]['content'])
        return response['data']['choices'][0]['content']
```


## Text2Vec
[text2vec](https://github.com/shibing624/text2vec)
[text2vec](https://pypi.org/project/text2vec/)
[PaddleNLP Embedding API](https://paddlenlp.readthedocs.io/zh/latest/model_zoo/embeddings.htm)

Text2vec: Text to Vector, Get Sentence Embeddings. Text vectorization, representing text (including words, sentences, paragraphs) as vector matrices.
text2vec implements multiple text representation and text similarity calculation models, including Word2Vec, RankBM25, BERT, Sentence-BERT, CoSENT, etc., and compares the effects of various models on text semantic matching (similarity calculation) tasks.
```py
pip install torch # conda install pytorch  2.0.1
pip install -U text2vec  # 1.2.1
```

## Text Vector Representation
Calculate text vectors based on pre-trained models
```py
>>> from text2vec import SentenceModel
>>> m = SentenceModel()
>>> m.encode("How to replace the bound bank card of Huabei")
//[-3.80448222e-01 -5.13956249e-01  4.24977601e-01 -3.34187746e-01
  4.87365782e-01 -4.59591091e-01  7.30618536e-01  2.61669636e-01
 -1.09546185e-01 -1.56673267e-01  1.00216138e+00  1.01447773e+00
  6.24635041e-01 -5.28593898e-01 ......]

# shibing624/text2vec-base-chinese is the default model specified by text2vec.SentenceModel
```

## Embedding
```py
import sys

sys.path.append('..')
from text2vec import SentenceModel
from text2vec import Word2Vec


def compute_emb(model):
    # Embed a list of sentences
    sentences = [
        'Card',
        'Bank Card',
        'How to replace the bound bank card of Huabei',
        'Huabei bound bank card replacement',
        'This framework generates embeddings for each input sentence',
        'Sentences are passed as a list of string.',
        'The quick brown fox jumps over the lazy dog.'
    ]
    sentence_embeddings = model.encode(sentences)
    print(type(sentence_embeddings), sentence_embeddings.shape)

    # The result is a list of sentence embeddings as numpy arrays
    for sentence, embedding in zip(sentences, sentence_embeddings):
        print("101 Sentence:", sentence)
        print("102 Embedding shape:", embedding.shape)
        print("103 Embedding head:", embedding[:10])
        print()


if __name__ == "__main__":
    # Chinese sentence vector model (CoSENT), recommended for Chinese semantic matching tasks, supports fine-tuning
    t2v_model = SentenceModel("shibing624/text2vec-base-chinese")
    compute_emb(t2v_model)

    # Multilingual sentence vector model (Sentence-BERT), recommended for English semantic matching tasks, supports fine-tuning
    sbert_model = SentenceModel("sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2")
    compute_emb(sbert_model)

    # Chinese word vector model (word2vec), suitable for Chinese literal matching tasks and cold start
    w2v_model = Word2Vec("w2v-light-tencent-chinese")
    compute_emb(w2v_model)
```

## moka-ai/m3e-base
[moka-ai]( https://huggingface.co/moka-ai/m3e-base)
```py
from sentence_transformers import SentenceTransformer

model = SentenceTransformer('moka-ai/m3e-base')

#Our sentences we like to encode
sentences = [
    '* Moka This text embedding model is trained and open-sourced by MokaAI, and the training script uses uniem',
    '* Massive This text embedding model is trained on a **million-level** Chinese sentence pair dataset',
    '* Mixed This text embedding model supports monolingual and bilingual text similarity calculation, heterogeneous text retrieval, etc., and will support code retrieval in the future, ALL in one'
]

#Sentences are encoded by calling model.encode()
embeddings = model.encode(sentences)

#Print the embeddings
for sentence, embedding in zip(sentences, embeddings):
    print("Sentence:", sentence)
    print("Embedding:", embedding)
    print("")    
```

## Sentence Similarity Calculation
```py
from text2vec import Similarity

# Two lists of sentences
sentences1 = ['How to replace the bound bank card of Huabei',
              'The cat sits outside',
              'A man is playing guitar',
              'The new movie is awesome']

sentences2 = ['Huabei bound bank card replacement',
              'The dog plays in the garden',
              'A woman watches TV',
              'The new movie is so great']

sim_model = Similarity()
for i in range(len(sentences1)):
    for j in range(len(sentences2)):
        score = sim_model.get_score(sentences1[i], sentences2[j])
        print("{} \t\t {} \t\t Score: {:.4f}".format(sentences1[i], sentences2[j], score))
//The sentence cosine similarity value score ranges from [-1, 1], and the larger the value, the more similar.        
```


## Gradio
[Documentation |](https://huggingface.co/learn/nlp-course/zh-CN/chapter9/7?fw=pt)
[Gradio Tutorial](https://blog.csdn.net/sinat_39620217/article/details/130343655)

In the previous part, we have explored and created demos using the Interface class. In this section, we will introduce our newly developed low-level API called gradio.Blocks.

What is the difference between Interface and Blocks?
    Interface: A high-level API that allows you to create a complete machine learning demo by providing input and output lists.
    Blocks: A low-level API that allows you to fully control the data flow and layout of your application. You can use blocks (like "building blocks") to build very complex multi-step applications.
```py
import gradio as gr

def flip_text(x):
    return x[::-1]

demo = gr.Blocks()

with demo:
    gr.Markdown(
        """
    # Flip Text!
    Start typing below to see the output.
    """
    )
    input = gr.Textbox(placeholder="Flip this text")
    output = gr.Textbox()

    input.change(fn=flip_text, inputs=input, outputs=output)

# demo.launch()
demo.launch(server_name='0.0.0.0', # ip for listening, 0.0.0.0 for every inbound traffic, 127.0.0.1 for local inbound
            server_port=7860, # the port for listening
            show_api=False, # if display the api document
            share=False, # if register a public url
            inbrowser=False)
