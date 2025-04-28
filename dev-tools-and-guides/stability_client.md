# <center>Stability AI</center>

## Download & Resources
[Official Website |](https://stability.ai/) 
[Documentation |](https://platform.stability.ai)
[REST-API |](https://api.stability.ai/docs)
[Pricing |](https://platform.stability.ai/pricing)
[Keyword Reference 1 |](https://mp.weixin.qq.com/s/Amo5PP9BqOWtWBCndWbXwQ)
[Keyword Reference 2 |](https://mp.weixin.qq.com/s/gzWnoP6MXTTukonpGQKIvQ)
[Prompt Generator |](http://sd.firstool.online/)

## Introduction
Stability AI provides powerful generative models and APIs for text-to-image and other creative AI tasks. Below are resources and examples for using the Stability AI REST API.

## REST API Example
[API Documentation](https://platform.stability.ai/rest-api)
```js
import fetch from "node-fetch"

let apiHost = 'https://api.stability.ai'
let API_KEY = "sk-lx8xxxxxxxxxx"
const response = await fetch(
    apiHost + '/v1/engines/list',
    {
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json',
          'Authorization': `Bearer ${API_KEY}`,
        },
        method: 'GET',
    }
)
const result = await response.json()
console.log(33, result)
}

// User endpoints
const urlAccount = `${apiHost}/v1/user/account`
const urlBalance = `${apiHost}/v1/user/balance`
// Model list endpoint
const urlEngines = `${apiHost}/v1/engines/list`

// Generation endpoint
`${apiHost}/v1/generation/${engineId}/text-to-image`,
https://api.stability.ai/v1/generation/{engine_id}/text-to-image

let engineId = 'stable-diffusion-xl-1024-v1-0'
let prompt = "A Yuezhou kiln dish designed with Plum Blossom abstract patterns, Rococo "

async function query() {
  const response = await fetch(
      `${apiHost}/v1/generation/${engineId}/text-to-image`,
      {
          headers: {
            'Content-Type': 'application/json',
            'Accept': 'application/json',
            'Authorization': `Bearer ${API_KEY}`,
          },
          method: "POST",
          body: JSON.stringify({
            text_prompts: [
              {"text": prompt}
            ],
            cfg_scale: 7,
            clip_guidance_preset: 'FAST_BLUE',
            height: 1024,
            width: 1024,
            samples: 1,
            steps: 30
          }),
      }
  )
  const result = await response.json()
  // result.artifacts[0].base64 contains the generated image (base64-encoded)
  // console.log(33, result)

  result.artifacts.forEach((image, index) => {
    let d = new Date()
    let time = d.getTime()
    fs.writeFileSync(
      `./out/v1_txt2img_${index}_${time}.png`,
      Buffer.from(image.base64, 'base64')
    )
    console.log(166, index, "generate ok")
  })
}

// Parameters:
// engine_id: string (e.g. stable-diffusion-v1-5)
// height: integer (default: 512)
// width: integer (default: 512)
// text_prompts: array of text prompts
// samples: integer (default: 1)
// seed: integer (default: 0)

## Combining with IPFS
```js
import fs from 'fs'
import fetch from "node-fetch"
import {create} from 'ipfs-http-client'

const ipfs = create({ host: 'example.io', port: '5002', protocol: 'https' })
const API_KEY = process.env.STABILITY_API_KEY
const stability_url = 'https://api.stability.ai'
const engineId = 'stable-diffusion-768-v2-1'
const ipfs_host = "https://example.io/ipfs/"

async function stability(prompt, id, username, num){
  const response = await fetch(
    `${stability_url}/v1/generation/${engineId}/text-to-image`,
    {
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json',
          'Authorization': `Bearer ${API_KEY}`,
        },
        method: "POST",
        body: JSON.stringify({
          text_prompts: [
            {"text": prompt}
          ],
          cfg_scale: 7,
          clip_guidance_preset: 'FAST_BLUE',
          height: 768,
          width: 768,
          samples: num,
          steps: 30
        }),
    }
  )
  const result = await response.json()
  // result.artifacts[0].base64 contains the generated image (base64-encoded)
  // console.log(88, result)

  let imgurls = []
  let img_length = result.artifacts.length
  for(let i = 0; i < img_length; i++){
    let content = Buffer.from(result.artifacts[i].base64, 'base64')
    let resX = await ipfs.add(content)
    let imgHash = resX.path
    // console.log(88, resX, 456, imgHash)
    imgurls.push(ipfs_host+imgHash)
  }
  return imgurls
}
```

Example prompt:
best quality, ultra-detailed, masterpiece, hires, 8k, stand up,girl,thin , short ponytail , red hair , smirk , fox ears , heart - shaped pupils , tail ,denim skirt,beautiful purple sunset at beach
