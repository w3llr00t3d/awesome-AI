# <center>ChatGPT</center>

ChatGPT is a conversational AI model developed by OpenAI. It is designed to generate human-like responses and assist with tasks such as answering questions, drafting emails, and more. This document provides an overview of the ChatGPT API, its usage, and practical examples.

## Download & Resources
- [Official Website](https://chat.openai.com/)
- [API Documentation](https://openai.com/blog/introducing-chatgpt-and-whisper-apis)
- [ChatGPT API](https://github.com/transitive-bullshit/chatgpt-api)
- [WeChat Chatbot](https://github.com/zhayujie/chatgpt-on-wechat)
- [Role-playing Prompts](https://github.com/f/awesome-chatgpt-prompts)
- [ChatGPT API Usage](https://zhuanlan.zhihu.com/p/610810300)

## Login Issues
- [Cloudflare Warp IP Restriction Removal](https://blog.larkneer.com/trend/@lemooljiang/7gt4ukb8)

## ChatGPT API
```js
const response = await openai.createChatCompletion({
    model: "gpt-3.5-turbo",
    messages: [{role: "user", content: "Hello world"}],
    max_tokens: 1500,
    temperature: 0.2
  })
console.log(156, "gpt", response.data.choices[0].message)

// parameters
model="gpt-3.5-turbo",
messages=[
      {"role": "system", "content": "You are a helpful assistant."},
      {"role": "user", "content": "Who won the world series in 2020?"},
      {"role": "assistant", "content": "The Los Angeles Dodgers won the World Series in 2020."},
      {"role": "user", "content": "Where was it played?"}
  ]

// curl call
this.axios.request({
  method: 'post',
  url: 'https://api.openai.com/v1/chat/completions',
  headers: {
    'X-Requested-With': 'XMLHttpRequest',
    'Authorization': 'Bearer your openai api key'
    },
  data:{
    model: "gpt-3.5-turbo",
    messages: [{"role": "user", "content": this.prompt}]
  }
})
.then(arg => {
  this.answer = JSON.parse(arg.request.response).choices[0].message.content
})

## Creating Context
Post-submit data, including all previous conversations, to form context.
```js
messages: [{"role": "user", "content": this.prompt}] 
// only one message without context

messages =  [
	{ 'role': 'user', 'content': 'Hello. Today is cloudy.' },
	{ 'role': 'assistant', 'content': 'Hello. Sorry to hear that the weather is not good.' },
	{ 'role': 'user', 'content': 'Yes, it is. But I am fine.' },
	{ 'role': 'assistant', 'content': 'I hope so. Let us continue with today\'s work!' },
	{ 'role': 'user', 'content': 'Yes. By the way, how did I describe today\'s weather?' },
	{ 'role': 'assistant', 'content': 'Today is cloudy' }
] 
// format previous conversation data into an array and pass it to ChatGPT

result = response["choices"][0]["text"].strip()
last_result = result
turns += [question] + [result]  # only this way can we ask follow-up questions and understand the context

// get the last 10 rounds of conversation
if len(turns) <= 10:  # to prevent exceeding the token limit, we only submit the last 10 rounds of conversation
    text = " ".join(turns)
else:
    text = " ".join(turns[-10:])
// limit input tokens

// text-davinci-003 model parameters are slightly different. It can only accept strings.
const response = await openai.createCompletion({
    model: "text-davinci-003",
    prompt: `${prompt}`,
    temperature: temperature, 
    max_tokens: 4000, 
    top_p: 1, 
    frequency_penalty: 0, 
    presence_penalty: 0, 
})
messages =  [
	{ 'role': 'user', 'content': 'Hello. Today is cloudy.' },
	{ 'role': 'assistant', 'content': 'Hello. Sorry to hear that the weather is not good.' },
	{ 'role': 'user', 'content': 'Yes, it is. But I am fine.' },
	{ 'role': 'assistant', 'content': 'I hope so. Let us continue with today\'s work!' },
	{ 'role': 'user', 'content': 'Yes. By the way, how did I describe today\'s weather?' },
	{ 'role': 'assistant', 'content': 'Today is cloudy' }
] 
// join array elements with `\n\n`
function getPreviousConversationContent(data) {
    let len = data.length
    let arr = [];
    for (var i = 0; i < len; i++) {
      let item = data[i]
      arr.push(item.content)
    }
    console.log(123, arr, "arr")
    return arr.join("\n\n")
  }
let s =  getPreviousConversationContent(messages)  
console.log(s, typeof s)