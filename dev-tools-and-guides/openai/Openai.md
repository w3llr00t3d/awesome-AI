# <center>OpenAI</center>

OpenAI is an artificial intelligence research laboratory that provides various AI models and tools for developers to build innovative applications. This document provides an overview of the OpenAI API, its usage, and examples.

## Download & Resources
[Official Website |](https://openai.com/)
[npm |](https://www.npmjs.com/package/openai)
[OpenAI Documentation |](https://platform.openai.com/docs/introduction)
[Node.js Library |](https://github.com/openai/openai-node)
[Python Cookbook |](https://github.com/openai/openai-cookbook)
[SMS Activation Platform |](https://sms-activate.org/en)
[Developer Reference |](https://github.com/adrianhajdin/project_openai_codex)
[Fine-tuning Models |](https://openai.com/blog/gpt-3-5-turbo-fine-tuning-and-api-updates)

## OpenAI API
[API Parameters |](https://platform.openai.com/docs/api-reference/chat/create)
```js
npm install openai --save  // "openai": "^4.16.1"
// npm update openai
npm install express cors --save

import express from 'express'
import cors from 'cors'
import OpenAI from "openai"

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
  baseURL: "https://example.com/v1"   // If you want to access through a proxy, add this for domestic use
})
//https://api.openai.com/v1/chat/completions
// Test:
curl https://example.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer sk-Axxxxx" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'

const app = express()
app.use(cors())
app.use(express.json())

app.get('/', async (req, res) => {
  res.status(200).send({
    message: 'Hello ilark AI!'
  })
})

// The model is gpt-3.5-turbo, which is the most cost-effective model
// temperature takes a value between 0 and 1, with higher values resulting in lower relevance
app.post('/gpt', async (req, res) => {
  try {
    const prompt = req.body.prompt
    const temperature = req.body.temperature

    const response = await openai.chat.completions.create({
        model: "gpt-3.5-turbo", 
        // messages: query,
        messages: [{"role": "system", "content": "You are a helpful assistant."},
                   {"role": "user", "content": "Who won the world series in 2020?"},
                   {"role": "assistant", "content": "The Los Angeles Dodgers won the World Series in 2020."},
                   {"role": "user", "content": "Where was it played?"}]
        temperature: 0.2, // Higher values means the model will take more risks.
        max_tokens: 1600, // The maximum number of tokens to generate in the completion. Most models have a context length of 2048 tokens (except for the newest models, which support 4096).
        top_p: 1, // alternative to sampling with temperature, called nucleus sampling
        frequency_penalty: 0, // Number between -2.0 and 2.0. Positive values penalize new tokens based on their existing frequency in the text so far, decreasing the model's likelihood to repeat the same line verbatim.
        presence_penalty: 0, // Number between -2.0 and 2.0. Positive values penalize new tokens based on whether they appear in the text so far, increasing the model's likelihood to talk about new topics.
      })

    console.log(55, response.data.choices[0].message)

    res.status(200).send({
      bot: response.data.choices[0].message
    });

  } catch (error) {
    console.error(error)
    res.status(500).send(error || 'Something went wrong');
  }
})
// Note: max_tokens here refers to the maximum output token value, not the model's max_tokens value! For example, gpt-3.5-turbo has a max_tokens value of 4096, but here max_tokens can only be set to 1600!
```

## GPT-4o
GPT-4o (“o” for “omni”) is our most advanced model. It is multimodal (accepting text or image inputs and outputting text), and it has the same high intelligence as GPT-4 Turbo but is much more efficient—it generates text 2x faster and is 50% cheaper. Additionally, GPT-4o has the best vision and performance across non-English languages of any of our models. GPT-4o is available in the OpenAI API to paying customers. Learn how to use GPT-4o in our text generation guide.

```js
import OpenAI from "openai"
import dotEnv from "dotenv"

dotEnv.config()
const apiKey = process.env.API_KEY

const Openai = new OpenAI({
  apiKey: apiKey
})

async function main() {
  const response = await Openai.chat.completions.create({
    model: "gpt-4o",
    messages: [
      {
        role: "user",
        content: [
          { type: "text", text: "What's in the picture" },
          {
            type: "image_url",
            image_url: {
              "url": "https://ipfs.ilark.io/ipfs/QmadtZxXPTVS9q2qArZHpZaRjYmF9o5HMxj6Hdgc59dGpR",
            },
          },
        ],
      },
    ],
  });
  console.log(65, response.choices[0])
}
```

## Python Version
[Reference |](https://platform.openai.com/docs/api-reference/chat/create?lang=python)
```py
pip install openai  # 0.27.8  1.14.2

from openai import OpenAI
from dotenv import dotenv_values
import sys

env_vars = dotenv_values('.env')

client = OpenAI(
  base_url="https://example.com/v1",
  api_key=env_vars['OPENAI_API_KEY'],
)

def aiChat(query):
    completion = client.chat.completions.create(
        model="gpt-3.5-turbo-0125",
        messages = query,
        temperature = 0.3
    )
    print(6662, completion.choices[0].message.content, file=sys.stderr)
    return completion.choices[0].message.content
```

## Embeddings
[youtube-gpt |](https://github.com/davila7/youtube-gpt)
[Chatbot |](https://github.com/FaustoNisida/Chatbot-Long-Short-Term-Memory)
[chatpdf |](https://github.com/postor/chatpdf-minimal-demo)
[Code a Project like ChatPDF |](https://postor.medium.com/how-to-code-a-project-like-chatpdf-e40441cb4168)
[Build an Intelligent Document Query |](https://mp.weixin.qq.com/s/bYxFySJEWPUHd2591jVHEQ)

Embeddings are a way to represent text as a numerical vector, which can be used to measure the relationship between two pieces of text. Our second-generation embedding model, text-embedding-ada-002, is useful for tasks such as search, clustering, recommendation, anomaly detection, and classification.

```js
const { Configuration, OpenAIApi } = require("openai");
const configuration = new Configuration({
  apiKey: process.env.OPENAI_API_KEY,
});
const openai = new OpenAIApi(configuration);
const response = await openai.createEmbedding({
  model: "text-embedding-ada-002",
  input: "The food was delicious and the waiter...",
});
//response  response.data.data[0].embedding

eg:
let jsonData = [
  {
    "text": "2022年1月10号到13号，俄罗斯分别与美国和北约开展对话.......",
    "embedding": [
      -0.012431818,
      -0.021277534, ......
      ]
  },
  {
    "text": "美国国防部宣布：将向欧洲增派部队，应对俄乌边境地区的紧张局势.....",
    "embedding": [
      -0.012431818,
      -0.021277534, ......
      ]
  }
]
// Find the similarity between two pieces of text
function cosineSimilarity(vecA, vecB) {
  let dotProduct = 0
  let normA = 0
  let normB = 0
  for (let i = 0; i < vecA.length; i++) {
      dotProduct += vecA[i] * vecB[i]
      normA += Math.pow(vecA[i], 2)
      normB += Math.pow(vecB[i], 2)
  }
  return dotProduct / (Math.sqrt(normA) * Math.sqrt(normB))
}

// Find the most similar text from the database
function getSimilarTextFromDb(inputEmbedding, jsonData) {
  let result = []
  jsonData.forEach(embedding => {
      let similarity = cosineSimilarity(inputEmbedding, embedding.embedding)
      // console.log("similarity", similarity)
      if (similarity > 0.8) {
          result.push({
              text: `${embedding.text}`,
              similarity: similarity
          })
      }
  })
  result.sort((a, b) => b.similarity - a.similarity)
  let topTwo = result.slice(0, 2)
  return topTwo.map(r => r.text).join("")
}

// 1. First, find the embedding vector of the text
// 2. Then, compare it with the pre-prepared database
// 3. Select the most similar text and concatenate it with the user's keyword for querying
const prompt = req.body.prompt  
const inputEmbeddingResponse = await openai.createEmbedding({       
  model: "text-embedding-ada-002",
  input: prompt
})
const inputEmbedding = inputEmbeddingResponse.data.data[0].embedding
const context = getSimilarTextFromDb(inputEmbedding, jsonData)
// console.log(289,"getSimilarTextFromDb", context)

let promptX = `I hope you can act as an expert on Russia and Ukraine issues. I will provide you with all the necessary information on Russia and Ukraine issues, and your responsibility is to answer my questions in simple and rigorous language.\n${context}\n${prompt}`
const response = await openai.createCompletion({
  model: "text-davinci-003",
  prompt: promptX,
  temperature: 0.2, 
  max_tokens: 3000, 
  top_p: 1, 
  frequency_penalty: 0, 
  presence_penalty: 0, 
})
```

## Streaming
Streaming is a way to process data in real-time, allowing for more efficient and interactive applications.

[Reference |](https://github.com/openai/openai-node/issues/18)
[node stream |](https://github.com/node-fetch/node-fetch#streams)
[ChatGPT Streaming Response |](https://juejin.cn/post/7222440107214241829)
[SSE Long Connection |](https://blog.csdn.net/m0_46672781/article/details/130296397)
[Stream |](https://juejin.cn/post/7249286903207641146)
```js
// Server-side implementation
try {
  res.setHeader('Cache-Control', 'no-cache')
  res.setHeader('Content-Type', 'text/event-stream')
  res.setHeader('Connection', 'keep-alive')
  res.flushHeaders()

  const stream = await openai.chat.completions.create({
    model: "gpt-4-1106-preview",
    messages: query,
    max_tokens: 1600,  // This is the maximum output token value, not the model's max_tokens value!
    temperature: temperature,
    stream: true,
  });
  for await (const chunk of stream) {
    // console.log(222,"chunk:", chunk)
    // console.log(126, "chunk:", chunk.choices[0]?.delta?.content)
    // process.stdout.write(chunk.choices[0]?.delta?.content || '');
    let strTemp = chunk.choices[0]?.delta?.content
    if(strTemp != null){
        console.log(658,"strTemp:", strTemp)
        outString += strTemp
        res.write(strTemp)
      }
  }
  console.log(444, "end")
  return res.end()
  } catch (error) {
    console.log(1112, error)
    return res.status(500).send('Something went wrong')
  }
///
{
  id: 'chatcmpl-8IdRXgCjduVw3JVCBGexhqUp5MbsJ',
  object: 'chat.completion.chunk',
  created: 1699452215,
  model: 'gpt-3.5-turbo-1106',
  system_fingerprint: 'fp_eeff13170a',
  choices: [ { index: 0, delta: [Object], finish_reason: null } ]
}
{
  id: 'chatcmpl-8IdRXgCjduVw3JVCBGexhqUp5MbsJ',
  object: 'chat.completion.chunk',
  created: 1699452215,
  model: 'gpt-3.5-turbo-1106',
  system_fingerprint: 'fp_eeff13170a',
  choices: [ { index: 0, delta: [Object], finish_reason: null } ]
}


// Client-side
let query = [{role: "user", content: data.get('prompt')}]
let dataObj = {
     method: 'POST',
     headers: {
          'Content-Type': 'application/json',
     },
     body: JSON.stringify({
          query
          })
}

const response = await fetch(url, dataObj)
let that = this
messageDiv.innerHTML = " "
if (response.ok) {	
let i = 0
let getStream = function (reader) {
     return reader.read().then(function (result) {
          // If the data has been read completely, return directly
          if (result.done) {
               console.log(889, "result done")
               that.clickFlag = false
               clearInterval(loadInterval)  
               loading.textContent = ''
               return
          }
          // Get the current segment of data (in binary format)
          let chunk = result.value
          let text = that.utf8ArrayToStr(chunk)
          if(i === 0){
               text = text.replace(/\\n/g,'')  // Remove the newline character at the beginning
          } else{
               text = text.replace(/\\n/g,'<br/>')
          }
          // console.log(5667, "i", i, text)
          // Append the current segment of data to the webpage
          messageDiv.innerHTML += text
          i ++
          // Recursively process the next segment of data
          return getStream(reader)
     })
}
getStream(response.body.getReader())  
```

## Count Tokens
[npm |](https://www.npmjs.com/package/gpt-3-encoder)
[OpenAI Tokenizer |](https://platform.openai.com/tokenizer)

GPT models process text using tokens. Tokens are common character sequences found in text. These models understand the statistical relationships between tokens and are good at generating the next token in a sequence.

A useful rule of thumb is that for ordinary English text, one token is approximately equal to 4 characters of text. This is roughly equivalent to 3/4 of a word, so 100 tokens ≈ 75 English words.
For Chinese text, 100 tokens ≈ 50 Chinese characters.

```js
cnpm install gpt-3-encoder --save  // "^1.1.4"

import {encode, decode} from 'gpt-3-encoder'

const str = '看了山和海，也看了人山和人海。'
const encoded = encode(str)
console.log(11, 'Encoded this string looks like: ', encoded) 
/*[
    40367,   233, 12859,   228,   161,
    109,   109,   161,   240,   234,
  38184,   115,   171,   120,   234,
  20046,   253, 40367,   233, 12859,
    228, 21689,   161,   109,   109,
    161,   240,   234, 21689, 38184,
    115, 16764
] */
console.log(12, 'Encoded lenght', encoded.length)  //32

console.log('We can look at each token and what it represents')
for(let token of encoded){
  console.log({token, string: decode([token])})
}

const decoded = decode(encoded)
console.log(22, 'We can decode it back into:\n', decoded)
```

## DALL-E
```js
async function main() {
  let response = await Openai.images.generate({
    model: "dall-e-3",
    prompt: "A red dress",
    n: 1,
    size: "1024x1024",
    response_format: "b64_json"
  })
  console.log(589, "dalle", response.data)
}
//response_format is not set, the default return is url
//response.data[0].url
//response.data[i].b64_json

// Parameters:
n： integer Optional  Defaults to 1,  Must be between 1 and 10.
size： string  Optional Defaults to 1024x1024,  Must be one of 256x256, 512x512, or 1024x1024.
response_format： string Optional Defaults to url , Must be one of url or b64_json
```

## Upload Image to IPFS
```js
//response.data[0].b64_json
let imgurls = []
let img_length = response.data.length
for(let i = 0; i < img_length; i++){
  let content = Buffer.from(response.data[i].b64_json, 'base64')
  let resX = await ipfs.add(content)
  let imgHash = resX.path
  // console.log(88, resX, 456, imgHash)
  imgurls.push(ipfs_host+imgHash)
}
```

## Read Image
Image models: GPT-4o, GPT-4o mini
```js
async function main() {
  const response = await Openai.chat.completions.create({
    model: "gpt-4o",
    messages: [
      {
        role: "user",
        content: [
          { type: "text", text: "What's in the picture" },
          {
            type: "image_url",
            image_url: {
              "url": "https://ipfs.ilark.io/ipfs/QmadtZxXPTVS9q2qArZHpZaRjYmF9o5HMxj6Hdgc59dGpR",
            },
          },
        ],
      },
    ],
  });
  console.log(65, response.choices[0])
}
```

## Calculate Image Tokens
There are two main ways to provide images to the model: by passing in an image URL or by passing in a base64-encoded image directly in the request.

Cost calculation
Like text input, image input is also measured and billed in tokens. The token cost of an image is determined by two factors: the image size and the detail option on each image_url block. All images with the "detail: low" option cost 85 tokens per image. "Detail: high" images are first resized to fit within a 2048 x 2048 square while maintaining the aspect ratio. Then, they are resized again so that the shortest side is 768px. Finally, we calculate how many 512px squares the image is composed of. Each square costs 170 tokens. The final count also includes an additional 85 tokens.

Here are some examples that illustrate this process.
A 1024 x 1024 square image in "detail: high" mode costs 765 tokens
1024 is less than 2048, so no initial resizing is needed.
The shortest side is 1024, so we resize the image to 768 x 768.
The image is composed of 4 512px squares, so the final token cost is 170 * 4 + 85 = 765.

```js
import url from 'node:url'
import https from 'node:https'
import { imageSize } from 'image-size'

async function getSize(imgUrl){
    const options = url.parse(imgUrl)
    return new Promise(resolve => {
        https.get(options, function (response) {
            const chunks = []
            response
                .on('data', function (chunk) {
                chunks.push(chunk)
                })
                .on('end', function () {
                const buffer = Buffer.concat(chunks)
                console.log(223, imageSize(buffer))
                resolve(imageSize(buffer))
                })
            })
    })
}


async function calImgToken(imgUrl) {
    let size = await getSize(imgUrl)
    console.log(556, size)
    let width = size.width
    let height = size.height
    console.log(669, width, height)
    let newWidth = 768
    let newHeight = 768
    let aspect_ratio = width / height 
    
    // First, resize the image to fit within a 2048 x 2048 square while maintaining the aspect ratio
    if (width > 2048 || height > 2048) {
      if (aspect_ratio > 1) {
        newWidth = 2048
        newHeight = parseInt(2048 / aspect_ratio)
      } else {
        newHeight = 2048
        newWidth = parseInt(2048 * aspect_ratio)
      }
    }

    // Second, resize the image so that the shortest side is 768px
    if (width >= height && height > 768) {
      newWidth = Math.floor((768 / height) * width)
    } else if (height > width && width > 768) {
      newHeight = Math.floor((768 / width) * height)
    }
    
    // Third, calculate how many 512px squares the image is composed of, and calculate the token cost
    const tiles_width = Math.ceil(newWidth / 512)
    const tiles_height = Math.ceil(newHeight / 512)
    const total_tokens = 85 + 170 * (tiles_width * tiles_height)
    console.log(569, total_tokens)
    return total_tokens
}
// calImgToken(1868, 892)  //1445
calImgToken("https://ipfs.ilark.io/ipfs/QmbwEHigZpVFNWrUJgP6u28t9NhAW2xbqUm7BMAzSEc2ND")
```

## Function Calling
Function calling can greatly enhance the capabilities of large language models, enabling them to perform tasks such as reasoning, information retrieval, database operations, knowledge graph search and reasoning, system operations, and external tool invocation.

![functioncall.jpg](https://ipfs.ilark.io/ipfs/QmSGU9RvhkxfGLNTznEK9pWhH8fCPSu3qWzuzZvm8RtNxA)

Function calling diagram

As shown above, in the second step, you can provide specific interface functions (i.e., external APIs) to add extra data to enhance the capabilities of large language models. In specific workflows, such as querying the weather or train schedules, large language models can accomplish these tasks.

Below is an example using OpenAI's sample code, which you can refer to after testing.

```js
import OpenAI from "openai"
import dotEnv from "dotenv"

dotEnv.config()
const apiKey = process.env.API_KEY

const openai = new OpenAI({
  apiKey: apiKey 
})

// Example dummy function hard-coded to return the same weather
// In production, this could be your backend API or an external API
function getCurrentWeather(location, unit = "fahrenheit") {
  if (location.toLowerCase().includes("tokyo")) {
    return JSON.stringify({ location: "Tokyo", temperature: "10", unit: "celsius" });
  } else if (location.toLowerCase().includes("san francisco")) {
    return JSON.stringify({ location: "San Francisco", temperature: "72", unit: "fahrenheit" });
  } else if (location.toLowerCase().includes("paris")) {
    return JSON.stringify({ location: "Paris", temperature: "22", unit: "fahrenheit" });
  } else {
    return JSON.stringify({ location, temperature: "unknown" });
  }
}

async function runConversation() {
  // Step 1: send the conversation and available functions to the model
  const messages = [
    { role: "user", content: "What's the weather like in San Francisco, Tokyo, and Paris?" },
  ];

  const tools = [
    {
      type: "function",
      function: {
        name: "get_current_weather",
        description: "Get the current weather in a given location",
        parameters: {
          type: "object",
          properties: {
            location: {
              type: "string",
              description: "The city and state, e.g. San Francisco, CA",
            },
            unit: { type: "string", enum: ["celsius", "fahrenheit"] },
          },
          required: ["location"],
        },
      },
    },
  ];


  const response = await openai.chat.completions.create({
    model: "gpt-3.5-turbo-0125",
    messages: messages,
    tools: tools,
    tool_choice: "auto", // auto is default, but we'll be explicit
  });
  const responseMessage = response.choices[0].message;
  console.log(111, "response", response)
  /*
  {
    id: 'chatcmpl-90oyNdEDQSxsvpMpwxZ3KeYShovtH',
    object: 'chat.completion',
    created: 1709982967,
    model: 'gpt-3.5-turbo-0125',
    choices: [
      {
        index: 0,
        message: [Object],
        logprobs: null,
        finish_reason: 'tool_calls'
      }
    ],
    usage: { prompt_tokens: 88, completion_tokens: 77, total_tokens: 165 },
    system_fingerprint: 'fp_4f0b692a78'
  }
  */

  console.log(123, "responseMessage", responseMessage)
  /* promptX
  {
  role: 'assistant',
  content: null,
  tool_calls: [
    {
      id: 'call_0QMDlATqYjBjgdySQVCL0PI4',
      type: 'function',
      function: [Object]
    },
    {
      id: 'call_e8ceipN3i2OUiF3Z38qNEEs5',
      type: 'function',
      function: [Object]
    },
    {
      id: 'call_cHlSgOIcsOXwpiOpzHIev6vF',
      type: 'function',
      function: [Object]
    }
  ]
  }
  */

  //console.log(396, "function", responseMessage.tool_calls[0].function)
  /*
  {
    name: 'get_current_weather',
    arguments: '{"location": "San Francisco", "unit": "celsius"}'
  }
  */

  // Step 2: check if the model wanted to call a function
  const toolCalls = responseMessage.tool_calls
  if (responseMessage.tool_calls) {
    // call the function
    // Note: the JSON response may not always be valid; be sure to handle errors
    const availableFunctions = {
      get_current_weather: getCurrentWeather,
    }; // only one function in this example, but you can have multiple
    messages.push(responseMessage); // extend conversation with assistant's reply
    console.log(225,"messages", messages)
    /*
    [{
      role: 'user',
      content: "What's the weather like in San Francisco, Tokyo, and Paris?"
    },
    {
      role: 'assistant',
      content: null,
      tool_calls: [ [Object], [Object], [Object] ]
    }]
    */

    for (const toolCall of toolCalls) {
      const functionName = toolCall.function.name;
      const functionToCall = availableFunctions[functionName];
      const functionArgs = JSON.parse(toolCall.function.arguments);
      const functionResponse = functionToCall(
        functionArgs.location,
        functionArgs.unit
      );
      messages.push({
        tool_call_id: toolCall.id,
        role: "tool",
        name: functionName,
        content: functionResponse,
      }); // extend conversation with function response
    }
    console.log(365,"messages2", messages)
  /*
  [{
    role: 'user',
    content: "What's the weather like in San Francisco, Tokyo, and Paris?"
  },
  {
    role: 'assistant',
    content: null,
    tool_calls: [ [Object], [Object], [Object] ]
  },
  {
    tool_call_id: 'call_0QMDlATqYjBjgdySQVCL0PI4',
    role: 'tool',
    name: 'get_current_weather',
    content: '{"location":"San Francisco","temperature":"72","unit":"fahrenheit"}'
  },
  {
    tool_call_id: 'call_e8ceipN3i2OUiF3Z38qNEEs5',
    role: 'tool',
    name: 'get_current_weather',
    content: '{"location":"Tokyo","temperature":"10","unit":"celsius"}'
  },
  {
    tool_call_id: 'call_cHlSgOIcsOXwpiOpzHIev6vF',
    role: 'tool',
    name: 'get_current_weather',
    content: '{"location":"Paris","temperature":"22","unit":"fahrenheit"}'
  }]
  */
    
    //Step 3: get a new response from the model where it can see the function response
    const secondResponse = await openai.chat.completions.create({
      model: "gpt-3.5-turbo-0125",
      messages: messages,
    }); 
    console.log(569, secondResponse)
    return secondResponse.choices;
  }
}


runConversation().then(console.log).catch(console.error);
```

After three steps, you will get the final result. Currently, the effect is not bad! **In fact, function calling (Function calling) and external vector databases have similar functions, both of which provide additional information to large language models to obtain more accurate results.** If you are developing a bot or agent, you can use it to expand the capabilities of large language models.

## Reverse Proxy
```py
# Overseas server nginx 
server { 
	server_name  example.com;

    location / {
        proxy_pass  https://api.openai.com/;
        proxy_ssl_server_name on;
        proxy_set_header Host api.openai.com;
        proxy_set_header Connection '';
        proxy_http_version 1.1;
        chunked_transfer_encoding off;
        proxy_buffering off;
        proxy_cache off;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

	error_page   500 502 503 504  /50x.html;
	location = /50x.html {
		root   /usr/share/nginx/html;
	}

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

# Client-side add one line api_base 
openai.api_base = "https://example.com/v1"
```

## Pricing
[pricing](https://openai.com/pricing)
$0.0200/1000tokens (approximately 750 English words), 1 token corresponds to approximately 4 characters.<br>
Image generation costs $0.016 per image.

ChatGPT API<br>
$0.002/1000tokens (approximately 750 English words)

Whisper API  <br>
$0.006/minute

GPT-4o
gpt-4o  US$5.00/1Mtokens (Input)  US$15.00/1M tokens(Output)

GPT-4 Turbo
gpt-4-turbo  US$10.00/1Mtokens (Input)  US$30.00/1M tokens(Output)

GPT-3.5 Turbo
gpt-3.5-turbo-0125  US$0.50/1Mtokens (Input)  US$1.50/1M tokens(Output)  1000000
