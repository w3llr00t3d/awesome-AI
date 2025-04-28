# <center>RWKV</center>

## Download & Resources
[Hugging Face |](https://huggingface.co/RWKV)
[Official Website |](https://www.rwkv.com)
[Paper |](https://arxiv.org/abs/2305.13048)
[GitHub |](https://github.com/BlinkDL/ChatRWKV)
[Desktop Installer |](https://github.com/josStorer/RWKV-Runner)
[Android Installer |](https://github.com/ZTMIDGO/RWKV-Android)
[RWKV Chat Model |](https://zhuanlan.zhihu.com/p/618011122)
[Chinese Novel Continuation Demo |](https://modelscope.cn/studios/BlinkDL/RWKV-CHN/summary)
[Fine-tuning Guide |](https://zhuanlan.zhihu.com/p/638326262)
[Local Deployment |](https://github.com/cgisky1980/ai00_rwkv_server/blob/main/README_zh.md)

## API Endpoints
https://better-chat-rwkv.ai-creator.net/
https://rwkv.ai-creator.net/jpntuned/v1/chat/completions
https://rwkv.ai-creator.net/chntuned/v1/chat/completions

## Introduction
RWKV is a recurrent neural network architecture that does not require attention mechanisms, making it faster and more memory-efficient. It also supports GPT-style parallel training. The model is open-sourced on Hugging Face and provides free API endpoints.

## Python Usage
```py
# Method 1: Using OpenAI-compatible API
import openai as RWKV
import sys

RWKV.api_base = "https://rwkv.ai-creator.net/chntuned/v1"
RWKV.api_key = ""

def rwkvChatX(query, temperature):
    completion = RWKV.ChatCompletion.create(
        model = "gpt-3.5-turbo",
        messages = query,
        temperature = temperature
    )
    print(6617, "completion", completion.choices[0].message.content)
    return completion.choices[0].message.content

# Method 2: Using requests
import requests

def send_get_request(prompt):
    try:
        url= "https://rwkv.ai-creator.net/chntuned/v1/chat/completions"
        message=[{"role": "user", "content": prompt}]
        data = {
            "messages":message,
            "temperature":0.3,
            "max_tokens":1500
        }
        headers = {'content-type': 'application/json', 'Authorization': 'Bearer '}
        response = requests.post(url=url, json=data, headers=headers)
        result = eval(response.text)
        response_text = result["choices"][0]["message"]["content"]
        return response_text
    except Exception as e:
        print("Request failed:", e)
        return None

response_data = send_get_request("Introduce yourself")
print(response_data)
```

## Node.js Usage
```js
// Method 1: Using OpenAI-compatible API
import OpenAI from "openai"

const RWKV = new OpenAI({  // RWKV-5-12B-one-state-chat-16k'
  apiKey: "",
  baseURL: "https://rwkv.ai-creator.net/chntuned/v1",
})

async function test(query){
    const response = await RWKV.createChatCompletion({
        messages: query,
        temperature: 0.2
      })
      console.log(55, response.data.choices[0].message)
}
let s = [{"role": "user", "content": "Introduce yourself"}]
test(s)

// Method 2: Using fetch
import fetch from "node-fetch"

const url= "https://rwkv.ai-creator.net/chntuned/v1/chat/completions"
async function test(){
	const response = await fetch(url, {
	  method: 'POST',
	  headers: {
		'content-type': 'application/json'
	  },
	  body: JSON.stringify({
	    messages: [
	      {"role": "system", "content": "You are a helpful assistant."},
	      {"role": "user", "content": "Introduce yourself"}
	    ],
	    "temperature":0.3,
	  })
	})
    const result = await response.text()
	const res = JSON.parse(result)
	console.log(566, res.choices[0].message.content)

}
test()