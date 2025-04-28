# <center>Midjourney API</center>

## Download & Resources
[npm |](https://www.npmjs.com/package/midjourney)
[GitHub |](https://github.com/erictik/midjourney-api)

## Introduction
Midjourney provides AI-powered image generation through Discord. The API simulates Discord calls. Before using, register on Discord, subscribe to Midjourney, and add the Midjourney bot to your server.

### Required Parameters
1. **Server ID & Channel ID:**
   - Find these in your Discord server URL:
     - Example: `https://discord.com/channels/1073xxx/10738xxx` (Server ID and Channel ID follow 'channels')
2. **User Token:**
   - [Reference](https://github.com/novicezk/midjourney-proxy/blob/main/docs/discord-params.md)
   - Open the channel, open network tools (F12), refresh (F5), find the 'messages' request, and copy the Authorization token.

## Installation
```bash
npm install midjourney --save  # e.g. ^4.3.13 or 4.3.17
# npm update midjourney --save
# or
yarn add midjourney
```

## Basic Usage
```js
import { Midjourney } from "midjourney"

const client = new Midjourney({
  ServerId: "xxxx",   // Server ID
  ChannelId: "xxxx",  // Channel ID
  SalaiToken: "xxxx", // User Token
  Debug: false,
  Ws: true,
})

await client.init()

async function main(){
  try {
    const prompt = "taken with Canon EOS 5D Mark IV, Canon EF 85mm f/1.2L II USM, ISO100, f/1.2, shutter speed 1/100; a flaming woman with wings, in the style of realistic yet stylized, dark orange and red, made of all of the above, i can't believe how beautiful this is, exaggerated poses, warmcore --ar 2:3"
    // Imagine
    const Imagine = await client.Imagine( prompt )
    console.log(669, Imagine)
    if (!Imagine || !Imagine.options) {
      console.log("no message")
      return
    }

    let imgarr = []
    for (let i = 0; i < Imagine.options.length; i++) { 
      let labels = ["U1", "U2", "U3", "U4"]
      if(labels.includes(Imagine.options[i].label)){
        let CustomID = Imagine.options[i].custom
        let Upscale = await client.Custom({
          msgId: Imagine.id,
          flags: Imagine.flags,
          customId: CustomID,
          content: prompt,
        })
        imgarr.push(Upscale)
      }
    }
    console.log(888, imgarr)
  } catch (e) {
    console.log("error", e)
  }
}
main()
