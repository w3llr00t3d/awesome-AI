## OpenRouter
[OpenRouter](https://openrouter.ai/docs)

OpenRouter provides access to multiple large language models and is accessible from within China. It offers a convenient way to interact with these models, making it an ideal choice for developers and researchers.

## Python Example
```py
import openai
from dotenv import dotenv_values
import sys

# Load environment variables from .env file
env_vars = dotenv_values('.env')
openai.api_base = "https://openrouter.ai/api/v1"
openai.api_key = env_vars['OPENROUTER_API_KEY']

# Function to generate a chat response
def routerChat(query, temperature):
    completion = openai.ChatCompletion.create(
        model = "openai/gpt-3.5-turbo",
        messages = [
            {"role": "system", "content": "You are a helpful assistant."},
            {"role": "user", "content": "Hello!"}
           ],
        temperature = temperature,
        headers={ "HTTP-Referer": "https://test.com",
                  "X-Title": "test" }
    )
    # Return the generated response
    return completion.choices[0].message.content


# Function to generate a streaming chat response
def routerStreaming(query, temperature):
    # Create a chat completion with streaming enabled
    response = openai.ChatCompletion.create(
        model = "openai/gpt-3.5-turbo-16k",
        messages = query,
        temperature = temperature,
        max_tokens = 2000,
        stream = True,
        headers={ "HTTP-Referer": "https://test.com",
                  "X-Title": "test" }
    )
    # Yield the generated response chunks
    for trunk in response:
        if trunk['choices'][0]['finish_reason'] is not None:
            data = '[DONE]'
            return 'ok', 200
        else:
            data = trunk['choices'][0]['delta'].get('content','')
        yield data
```

## JavaScript Example
```js
// Method 1: Using the OpenAI API client library
import { Configuration, OpenAIApi } from 'openai'

// Create a configuration object with API key and base path
const configuration = new Configuration({
  apiKey: "sk-or-xxxxxxx",
  basePath: "https://openrouter.ai/api/v1",
  baseOptions: {
    headers: {
        "HTTP-Referer": "https://test.com",
        "X-Title": "test"
    }
  }
})

// Create an instance of the OpenAI API client
const openai = new OpenAIApi(configuration)

// Function to generate a chat response
async function test(query){
    // Create a chat completion with the specified model and query
    const response = await openai.createChatCompletion({
        model: "openai/gpt-3.5-turbo", 
        messages: query,
        temperature: 0.2, 
        max_tokens: 1500, 
        top_p: 1, 
        frequency_penalty: 0, 
        presence_penalty: 0, 
      })
      // Log the generated response
      console.log(55, response.data.choices[0].message)
}
// Test the function with a sample query
let s = [{"role": "user", "content": "Hello!"}]
test(s)

// Method 2: Using the Fetch API
import fetch from "node-fetch"

// Set the API key
const apiKey = "sk-xxxxxxx"

// Function to generate a streaming chat response
async function test(){
	// Create a POST request to the chat completions endpoint
	const response = await fetch("https://openrouter.ai/api/v1/chat/completions", {
	  method: 'POST',
	  headers: {
	    'Authorization': 'Bearer ' + apiKey,
	    'HTTP-Referer': "https://test.com", 
	    'X-Title': "test"
	  },
	  body: JSON.stringify({
	    model: "openai/gpt-3.5-turbo", 
	    messages: [
	      {"role": "system", "content": "You are a helpful assistant."},
	      {"role": "user", "content": "hello"}
	    ],
	    stream:true
	  })
	})

	// Log the generated response chunks
	try {
		for await (const chunk of response.body) {
		console.log(566,chunk.toString())
		}
	} catch (err) {
		console.error(err.stack)
	}

}
// Test the function
test()