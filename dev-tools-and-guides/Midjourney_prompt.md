# <center>Midjourney Prompt Guidelines</center>

## Introduction
This guide provides tips and examples for crafting effective prompts for Midjourney, an AI-powered image generation tool. Well-structured prompts can help you achieve more accurate and creative results.

## Download & Resources
[Official Website](https://www.midjourney.com/)
[Documentation](https://docs.midjourney.com/docs/quick-start)
[Style Reference](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference)
[Art Styles](https://midlibrary.io)
[Art Styles](https://lib.kalos.art/)
[Openjourney](https://replicate.com/prompthero/openjourney/api#run)
[Replicate Doc](https://replicate.com/docs)
[PromptHero](https://prompthero.com/)
[Keyword Assistant](https://prompt.noonshot.com/midjourney)
[Huggingface Prompt](https://huggingface.co/spaces/doevent/prompt-generator)
[Prompt Generator](https://www.howtoleverageai.com/midjourney-prompt-generator)
[Keyword Collection](https://zhuanlan.zhihu.com/p/614050846)
[Complete Commercial Design Tutorial](https://u0gp5ergxyk.feishu.cn/wiki/WMPkwaYybi5X1ykqyZHcFPeAnue)

## Keyword Format
![Basic Format](https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ_Prompt_basic.png)
/imagine prompt 

![Keyword Format ](https://cdn.document360.io/3040c2b6-fead-4744-a3a9-d56d621c6c7e/Images/Documentation/MJ%20Prompt.png)
/imagine url prompt parameters   //You can use an image as a keyword
Example:  https://s.mj.run/4t043-II22c subway station 

## Basic Commands
/imagine prompt Generate image<br>
/blend Combine, e.g. combine two images <br>
/info User information<br>
/settings Settings
/describe Generate prompts from images (reverse engineering)
/subscribe Manage subscription

## Prompt Format
Subject + Perspective + Distance + Mood + Details + Lighting + Style + Parameters
Example: character of monkey in style of nothing see nothing hear nothing speak wearing headphones listening rap, citylights background, Hyper detailed, hyper realistic, 8k, --ar 9:16 --s 950
Monkey character, sees nothing, hears nothing, wearing headphones listening to rap, city lights background, ultra-detailed, ultra-realistic, 8k-v 5.1-ar 9:16-s 950

Main subject + Atmosphere/Lighting/Color + Composition + Style Reference
Example: A spaceship surrounded by a swarm of small red craft, foggy, top-lit, strongly reflective, wide-angle, ultra-high-definition detail, concept art
A spaceship surrounded by a group of small red flying craft, foggy, top-lit, highly reflective, wide-angle, ultra-high-definition detail, concept art

## Common Parameters
--ar(aspect) Image aspect ratio, e.g. --ar 16:9  --ar 5:4<br>
--q (quality) 0.25-5 Quality parameter, default is 1, the higher the value, the higher the quality<br>
--s(stylize) 100-1000, degree of stylization, default is 100<br>
--c(chaos) 0-100 default is 0, initial image variation<br>
--v(version) Version number, if set in settings, no need to set again<br>
--seed For continuous characters, reference specific artworks<br>
--niji 5 Anime mode<br>
--iw 0-2 Reference image weight, used in blending<br>
--no Remove certain elements, e.g. --no white, no white in the image<br>
--stop 0-100 Stop generating images at a certain point<br>
--panels Continuous action<br>
--tile Repeating pattern image

## Image Parameters Tips
1. First generate a cool background, abstract tron legacy light rays
2. Then image + subject, https://s.mj.run/Yft37s2rrN0 cyberpunk adorable kitten
  You can also use two images to blend (blend)

## Adding Text
Currently only supports English, use "" to enclose, add keyword --style raw, e.g. "harry Potter --style raw"

## Common Keywords
1. Sticker Design --- Sticker style eg: sticker design of cute girl <br>
  Graphic Design of robot and flowers <br>
  Graphic Design, A phone is rising, surrounded by lights and flowers<br>
  Industrial Design, Hard edge style phone<br>
  Graphic Logo Design,Star-shaped, Simple, line construction<br>
2. "A object" As "B character"  --- Character replacement eg: elon mask as a commander<br>
3. Symmetrical, flat icon design --- Simple, symmetrical logo design eg: lemon, Symmetrical, flat icon design <br>
4. Game sheet of --- Game equipment list eg: game sheet of gens <br>
5. Knolling --- Organizing related objects in a parallel or 90-degree arrangement  eg: knolling tool set， knolling tool fruits<br>
6. 8-bit, 16-bit  --- Retro game, pixel art eg: 8-bit game pixel art, star war <br>
7. _ out of [material ]  --- Object covered in material eg: castle out of flowers <br>
8. Layered Paper  --- Paper folding art style eg: layered paper sea wave<br>
9. Isometric art  --- Isometric art style eg: Isometric tower<br>
10. Blacklight --- Black light effect eg: blacklight bridge<br>
11. Naïve art --- Naïve art style eg: naive art ant<br>
12. Mascot Logo --- Mascot design <br>
  eg: Mascot Logo carribage<br>
      A Yuezhou kiln, Mascot Logo<br>
13. T-shirt vector --- Clothing design eg: T-shirt vector dogs and flowers<br>
14. Pattern --- Pattern design  <br>
  eg: chinese native pattern<br>
  A Yuezhou kiln dish designed with Chinese cloud abstract patterns <br>
  A Yuezhou kiln dish designed with fish abstract patterns
15. Tattoo --- Tattoo design eg: rose tatton design<br>
16. Interior Design，architecture --- Building design eg: Interior Design， a warm chinese house<br>
17. Photorealistic --- Photorealistic design eg: a red car in forest, Photorealistic 
18. Stained glass window --- Window covered in objects eg: flower Stained glass window 
19. Blender 3D --- 3D effect eg: a wood horse Blender 3D<br>
20. Explode_____by Nychos -- Explosive street art eg: Explode planet by Nychos
21. logo for __
  Logo design for a copany called H.M modern design
  Design a logo for travel agency, in the style of paul rand
  Piza, symmertical flat icon design
  Yuezhou kiln, simple, as a logo
22. Long Exposure Long exposure
23. POV （point of view）First-person perspective
24. Elegant, 5000s 
 eg: a beautiful girl, Elegant, 5000s,front view --ar 16:9 
25. Art Nouveau New Art   Rococo洛可可

## Splitting Keywords
Use `|`、`::` to split keywords, e.g.: hot ｜ dog， hot:: dog
It will be accepted as two keywords, rather than understanding it as a single word "hot dog" .

Weight:
You can also set the weight of keywords, 1-4, default is 1, e.g.: 
  hot::2 dog 
  shopping mall::12  by teamlab::30

## Remixing
Bias towards random artistic effects
/prefer remix  or /settings Remix to enable or disable

## Artist Style
style of __ 
  Van Gogh
  Zhang Xiaogang
  Hayao Miyazaki
  Takashi Murakami
  Yayoi Kusama
eg:
A kite flying in the sky, a boy and a girl chasing on the ground, a lot of flowers on the grass, low shrubs,style of Yayoi Kusama<br>
A few shrimps chased by a crab，ink painting style of Murakami Takashi <br>
A few shrimps chased by a crab:: picasso::1.4<br>
A few shrimps chased by a crab, Painted By Andre Masson<br>

## Super Realism
[Realism](https://www.reddit.com/r/midjourney/comments/119mwu2/i_entered_every_fancy_keyword_that_i_knew_of_to/)
```  
prompt:
portrait of an indian village woman in forest in Himachal pradesh, clear facial features, Cinematic, 35mm lens, f/1.8, accent lighting, global illumination --uplight

portrait of an peking girl, clear facial features, Cinematic, 35mm lens, f/1.8, accent lighting, global illumination --uplight 
```

## Lighting
1. Double Exposure: double exposure 
   eg: a beautiful girl and flowers, double exposure
2. Cinematic Lighting: cinematic lighting
3. Dark Lighting: dark moody lighting 
   eg: blacklight bridge, dark moody lighting 
4. Fairy Lighting: fairy light
5. Holography: holography
6. Rembrandt Light	伦勃朗光
7. Mood Lighting	情绪照明
8. Soft Illumination/ soft lights	柔和的照明/柔光

## View
Aerial view	鸟瞰图
andala	曼茶罗构图
ultrawide shot	超广角
extreme closeup	极端特写
macroshot	微距拍摄
an expansive view of	广阔的视野
busts	半身像
profile	侧面
symmetrical body	对称的身体
symmetrical face	对称的脸
wide view	广角
bird view 俯	视/鸟瞰
up view	俯视图
front view	正视图
symmetrical	对称
Center the composition	居中构图
symmetrical the composition	对称构图

## Design Style
1. ASCII Art: ASCII art 
  eg: ASCII art, a girl face ,front view
2. Collage Art: collage art
3. Op Art (Optical Art): Op art 
  eg:a beautiful girl face, Op art 
4. Synthwave: synthwave
 eg: a beautiful woman, Smiling, front view, synthwave
5. Retrowave: Retrowave  eg: a beautiful woman, Smiling, Retrowave 
6. Watercolor: Watercolor sketch of a boy
 eg: Watercolor sketch of peking street
7. Plasticine: Plasticine
  eg:Plasticine of a cat
8. Inspired: inspired
9. Anthropomorphize: Anthropomorphize
10. Cloisonnism: Cloisonnism
   eg:Cloisonnism style porcelain of a bottle
      Yuezhou Kiln of a bottle
11. Fairy Kei Fashion: Fairy Kei fashion  
  eg: a beautiful girl in red, Fairy Kei fashion

## Style Avatar
Image + Style + Parameters, eg:
https://ipfs.ilark.io/ipfs/QmRobehozeey31UW7cCDazoikqTXi1a7aZd2vERvXx3jsm portrait, style of Van Gogh --iw 1.1

## Logo Design
[Logo Design Tutorial](https://mp.weixin.qq.com/s/8DUXnEy0EYxvx0rw3p9ZXw)

 Logo type + Main subject + Style + Artist + Perspective + Industry + Color 

Single-color Black and White Logo
Company logo, flat, clean, simplicity modern, minimalist, vintage, cartoon, geometric, lettermark logo of letter MS
公司LOGO，扁平、干净、简约、现代、极简主义、复古、卡通、几何、字母标记LOGO，字母为MS。

Letter-based Logo
A company logo, stylized capital letter B, playground theme, overall purple color scheme
公司标志，以大写字母B为主题，运动场景，整体紫色调

letter A Logo, Logo design, flat vector, minimalist --s 200
字母A标志，标志设计，平面矢量，极简主义

Mascot Logo
提示词：red fox, side view, Logo design, minimalist, color block, black background
翻译：赤狐，侧视图，标志设计，极简主义，色块，黑色背景

Line Logo
提示词：fox, front view, Logo design, minimalist, line, soft, white background
翻译：狐狸，前视图，标志设计，极简主义，线条，柔和，白色背景

Hand-drawn Logo
提示词：a coffee shop handwriting style Logo, there’s a little bird, Simplicity
翻译：一个咖啡店 手写风格的标志，有一只小鸟，简单

Specific Graphics
提示词： Cloud, Logo design, minimalist Logo, dazzling light, sparkling feeling, gradient colors, psychedelic
翻译：云，标志设计，极简主义标志，耀眼的光线，闪闪发光的感觉，渐变色，迷幻色，明亮的颜色

Combining Multiple Graphics
Logo composed of cloud and game controller, Logo design, minimalist logo, gradient colors
由云朵和游戏手柄组成的标志，标志设计，极简主义标志，渐变色

Abstract Graphics
提示词：simple linear wavy Logo, minimalistic
简单的线性波浪形标志，极简

Badge-style
提示词：red fox, front view, Logo design, badge, black background
翻译：赤狐，前视图，标志设计，徽章，黑色背景
提示词： （图像提示）elephant, front view, Logo design, badge,
翻译：大象，正面视图，标志设计，徽章，黑色背景，圆形

Industry Logo
提示词：The Logo of a coffee shop
提示词：Logo of a technology company

提示词： a logo for a children’s toys brand, simple,vector,by Pablo Picasso
儿童玩具品牌的标志，简单，矢量，毕加索风格


Black and White Graphics Logo
A simple black and white creative logo drawn with ONLY ONE SINGLE LINE representing a stylised Wolf from profil in a minimalistic oneline tattoo style, 2D, Harmonious, full body 
一个简单的黑白创意标志，用一条线条勾勒出了一个符号化的侧面狼形象，在极简主义的单线纹身风格中，2D，和谐，全身

Multi-color Graphics Logo
create very simple logo  for company named MANTE with many colors 
为名为“MANTE”的公司设计非常简单的标志，使用多种颜色。

Flat Graphics Logo
Logo design, a logo that combines coffee related elements, vector, flat and simple design, with a white background, designed by Alvin Lustig 
标志设计，一个结合咖啡相关元素的标志，矢量图，扁平而简约的设计，白色背景，由阿尔文·拉斯蒂格设计
eg: Logo design, a logo that combines yuezhou kiln related elements, Mainly celadon design and development,vector, flat and simple design, with a white background, designed by Paul Rand 
eg2: Logo design, a logo that combines Hunan Tourism Development Conference related elements, reflecting Yueyang's culture and elements,vector, flat and simple design, with a white background, designed by Paul Rand 


Designer:
Paul Rand (1914-1996)
Alvin Lustig (1915-1955)
Alexander Alex Steinweiss (1917-2011)
Bradbury Thompson (1911-1995)
George Tscherny (1924- )
Picasso style: Pablo Picasso

## Mascot Design
Image generation formula: Type + Main subject + Style + Color + Perspective + Texture/Lighting  

//Animal Image
提示词： three views, front view, side view, back view, cartoon IP, orange cat, full body, standing, clean background, pixar, IP blind box clay material, studio light, octane render, 3D, C4D, blender, hyper quality, UHD, 8K --ar 16:9 --niji 5
翻译： 三视图，前视图，侧视图，后视图，卡通IP，橙色猫，全身，站立，干净的背景，皮克斯，IP, 盲盒黏土材料，工作室灯光，辛烷值渲染，3D，C4D，blender（一款3D软件），超高质量，超高清, 8K

提示词： 3D animation style character design, a cute polar dog cartoon character, --niji 5 --s 120
翻译： 3D动画风格的人物设计，一个可爱的北极犬卡通人物

//Animal Anthropomorphism
提示词： 3D, chibi, a cute tiger cartoon character, holding a book, wearing an overcoat, front view, blue and pink, pure white background, POP MART style, IP image, advanced natural color matching, cute and colorful, exquisite details, C4D, octane renderer, ultra high definition, perfect lighting, cartoon realism,fun character settings, ray tracing --niji 5 --ar 16:9 --s 120
翻译： 3D，chibi，一个可爱的老虎卡通人物，拿着一本书，穿着大衣，正视图，蓝色和粉红色，纯白背景，泡泡玛特风格， IP形象，高级自然配色，可爱多彩，精致的细节， C4D，辛烷值渲染，超高清，完美照明，卡通现实主义，有趣的角色设置，光线追踪

//Ancient Style Character
提示词： a super cute girl, wearing traditional Chinese Hanfu, chibi, dreamy, IP character design, full body,white background, bright color, Pixar style, 3D render, front lighting, high detail, C4D, 8K
翻译： 一个超级可爱的女孩，穿着中国传统汉服，chibi，梦幻，IP人物设计，全身，白底，明亮的颜色，皮克斯风格，3D渲染，正面照明，高细节，C4D，8K


## Emoji Pack
[img url] a young man,emoji pack, multiple poses and expressions,[happy,sad,expectant,laughing,disappointed,surprised,pitiful,aggrieved,despised,embarrassed,unhappy] 2d,meme art,white background --niji 5 --iw 1.1

## Character Consistency
Consistent with previous style --sref naming is basically consistent, --cref.

We can use multiple URLs to combine information/characters from multiple images, e.g. --cref URL1 URL2 (similar to multiple images or style prompts).
eg: Someone sitting in the middle of a concert --cref https://s.mj.run/rvDu3RXchC8

eg2: You are sitting in a chair, sipping coffee, with a beautiful robot serving you beside you. Other robots are busy working in the background. The scene is captured from a first-person perspective, showcasing the futuristic environment filled with advanced technology and the elegance of the robot, --cref https://s.mj.run/V2kCoCMTPbo --cw 0

eg3: SURREAL "TTAI" Vintage Analog dub chanson vinyl music album cover "Welcome to TagAI" music album cover, editorial nerdy woman sitting on a vintage couch focused on embroidering a blue baseball caps with white threads, she is wearing blue and white and red starred onesie and has long dark hair with bangs. There's an embroidered blue baseball cap on her lap that says "TTAI". There are more blue baseball caps scattered around her on the couch, in front of vintage red and white wallpaper. Murica MASA warning blueprint report, style of Miyazaki Hayao --chaos 60 --ar 16:9 --sref 1390278553 --stylize 800

--cw parameter function
Midjourney can identify character attributes from reference images and mix them with prompts to create new characters. You can control this using the --cw N parameter (cref weight), where N can be 1-100. The default value is 100.
--cw parameter function:
--cw 100 (default) captures the entire character;
--cw 0 captures only the face, similar to face replacement, and you cannot turn off face reference

## Art Style
watercolor， 水彩画
Psychedelic，迷幻风格
Chinese ink style, ink drop，水墨画
graphite sketch，素描
tie dye illustration，扎染风格
Cubist screen print illustration style，立体拼贴风格
black line art work,black and white，黑白色调
in pop art retro comic style，波普艺术
Charcoal drawing，炭笔画
Pointillism,in style of Georges Seurat，点彩画
woodcu print，木刻画
oil painting,brush strokes，油画
