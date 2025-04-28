# <center>HuggingFace</center>

## Download & Resources
[Official Website |](https://huggingface.co)
[API Documentation |](https://huggingface.co/docs/api-inference/index)
[Dashboard |](https://api-inference.huggingface.co/dashboard/usage)

## Introduction
Hugging Face provides a wide range of open-source models and APIs for NLP, computer vision, and more. Below are examples for using the Hugging Face API in JavaScript.

## API Example (JavaScript)
```js
// Install node-fetch if not already installed:
// npm install node-fetch --save

import fetch from "node-fetch"

// Replace <MODEL_ID> with your desired model, e.g. "succinctly/text2image-prompt-generator"
async function query(data) {
  const response = await fetch(
      'https://api-inference.huggingface.co/models/succinctly/text2image-prompt-generator',
      {
          headers: { Authorization: `Bearer ${API_TOKEN}` },
          method: "POST",
          body: JSON.stringify(data),
      }
  )
  const result = await response.json();
  console.log(22, result[0].generated_text)
  // return result;
}
query(prompt)

//axios version
async function query(prompt){
  let res = await axios.request({
    url: 'https://api-inference.huggingface.co/models/succinctly/text2image-prompt-generator',
    headers: { Authorization: `Bearer ${API_TOKEN}` },
    method: "POST",
    data: JSON.stringify(prompt),
  })

  // console.log(123, res) 
  console.log(123, res.data[0].generated_text) 
}

query(prompt)
```