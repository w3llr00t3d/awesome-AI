# <center>ZhipuAI</center>

## Download & Resources
[GitHub |](https://github.com/THUDM/ChatGLM-6B)
[GitHub2 |](https://github.com/THUDM/ChatGLM2-6B)
[Official Website |](https://chatglm.cn/)
[ZhipuAI Platform |](https://open.bigmodel.cn)
[API Documentation |](https://open.bigmodel.cn/doc/api)
[Course |](https://huggingface.co/learn/nlp-course/zh-CN/chapter0/1?fw=pt)
[Local Knowledge Base |](https://github.com/imClumsyPanda/langchain-ChatGLM)
[Local Knowledge Base 2 |](https://github.com/thomas-yanxin/LangChain-ChatGLM-Webui)
[DB-GPT |](https://github.com/csunny/DB-GPT)
[ChatLongDoc |](https://github.com/webpilot-ai/ChatLongDoc)
[Build a Personal Knowledge Base with ChatGLM-6B |](https://baijiahao.baidu.com/s?id=1765950735847976093&wfr=spider&for=pc)
[Installation Guide |](https://baijiahao.baidu.com/s?id=1769236478823582890&wfr=spider&for=pc)
[ChatGLM-6B Deployment and P-Tuning Fine-tuning Practice](https://baijiahao.baidu.com/s?id=1765123631287305087)
[Video Tutorial](https://www.bilibili.com/video/BV1414y1m7mE)
[Parse Novels with Langchain-ChatGLM |](https://mp.weixin.qq.com/s/6TckgOO3ZKS9lDhHOq5h0A)
[Deep Learning of Langchain-ChatGLM Middleware |](https://mp.weixin.qq.com/s/m6JZvUPU2lzRSPlbKtXABA)
[Integrate into LangChain |](https://juejin.cn/post/7226157821708681277)
[Notes on LLM QA |](https://zhuanlan.zhihu.com/p/627439522)

## Introduction
ChatGLM-6B is a bilingual (Chinese-English) conversational language model with 6.2 billion parameters. It is pre-trained on a dataset of over 100 billion words, including news, encyclopedias, social media, novels, movie scripts, and especially dialogue data such as TV scripts, chat logs, and Q&A platforms. This diverse dataset enables ChatGLM-6B to learn comprehensive and in-depth language knowledge and adapt to different conversation styles and topics.

With these optimizations, ChatGLM-6B can be deployed locally on consumer-grade graphics cards and enables real-time conversational interaction. The model can run with as little as 6GB of VRAM at INT4 quantization and achieve inference speeds of 10 sentences per second (20 words per sentence) on an RTX 3090.

## Environment Setup
```
# Python 3.10.6, pip 23.1.2
virtualenv glm_env
source glm_env/bin/activate
# On Windows:
source glm_env/Scripts/activate
```

## Installation
```py
pip install zhipuai   # 1.0.7 or 2.0.1
# pip install zhipuai  -i https://pypi.tuna.tsinghua.edu.cn/simple
# Upgrade
pip install --upgrade zhipuai 
```

## Basic Usage
```py
from zhipuai import ZhipuAI
from dotenv import dotenv_values
import sys
import json

env_vars = dotenv_values('../.env')
client = ZhipuAI(api_key=env_vars['ZHIPU_API_KEY'])

def zhipuChatV2():
    response = client.chat.completions.create(
        model = "glm-4",
        messages = [{"role": "user", "content": "Artificial Intelligence"}],
        top_p = 0.7,
        temperature = 0.9,
    )
    print(33, response, file=sys.stderr)
    print(366, response.choices[0].message, file=sys.stderr)
    return response.choices[0].message.content
```

## Function Call
```py
from zhipuai import ZhipuAI
import json

client = ZhipuAI(api_key="xxx")

def query_train_info(date, departure, destination):
    # This is a test API call, replace with actual implementation
    print(6689, "query_train_info", date, departure, destination)
    return "Beijing-Guangzhou-9966"

def parse_function_call(model_response, messages):
    # Process function call results, call corresponding functions based on model response
    print(556, model_response.choices[0].message)
    if model_response.choices[0].message.tool_calls:
        tool_call = model_response.choices[0].message.tool_calls[0]
        args = tool_call.function.arguments
        function_result = {}
        if tool_call.function.name == "query_train_info":
            function_result = query_train_info(**json.loads(args))
        messages.append({
            "role": "tool",
            "content": f"{json.dumps(function_result)}",
            "tool_call_id": tool_call.id
        })
        response = client.chat.completions.create(
            model="glm-4",
            messages=messages
        )
        print(666, response.choices[0].message)
        # messages.append(response.choices[0].message.model_dump())

def test_func_call():
    messages = []   
    messages.append({"role": "system", "content": "Do not assume or guess the input parameters of the function. If the user's description is unclear, ask the user to provide necessary information"})
    messages.append({"role": "user", "content": "Help me query the train information for January 23rd from Beijing to Guangzhou"})
    tools = [
        {
            "type": "function",
            "function": {
                "name": "query_train_info",
                "description": "Query train information based on departure, destination, and date",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "departure": {
                            "description": "Departure city",
                            "type": "string"
                        },
                        "destination": {
                            "description": "Destination city",
                            "type": "string"
                        },
                        "date": {
                            "description": "Date",
                            "type": "string",
                        }
                    },
                    "required": [ "departure", "destination", "date" ]
                },
            }
        }
    ]
     
    response = client.chat.completions.create(
        model="glm-4",
        messages=messages,
        tools=tools,
    )
    print(112, response.choices[0].message)
    messages.append(response.choices[0].message.model_dump())
    parse_function_call(response, messages)

if __name__ == "__main__":
    test_func_call() 

112
content=None role='assistant' tool_calls=[CompletionMessageToolCall(id='call_8477880785126313244', function=Function(arguments='{"date":"2022-01-23","departure":"Beijing","destination":"Guangzhou"}', name='get_flight_number'), type='function')]

556
556 content=None role='assistant' tool_calls=[CompletionMessageToolCall(id='call_8477880819486041138', function=Function(arguments='{"date":"2022-01-23","departure":"Beijing","destination":"Guangzhou"}', name='query_train_info'), type='function')]

6689 query_train_info 2022-01-23 Beijing Guangzhou
666 content='According to your request, I queried the train information for January 23rd from Beijing to Guangzhou, and the train number is "Beijing-Guangzhou-9966". Please note that this is a train number, and you need to check the specific train schedule, airline, and other details through the airline or train inquiry service. If you need more help, please let me know.' role='assistant' tool_calls=None    
```

## Streaming
[Streaming ChatGPT Interface |](https://blog.csdn.net/time_forgotten/article/details/130437413)
[GitHub |](https://github.com/wemio/chatGPTFlaskWebAPI)
```py
# SSE response is a string stream format, see the specific response example
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


## Vector Calculation
```py
from zhipuai import ZhipuAI

client = ZhipuAI(api_key="xxxx")

response = client.embeddings.create(
    model="embedding-2", # Fill in the model name to be called
    input="Hello",
)

print(662, response.data[0].embedding)
