
##  Chinese Instruction Multimodal Data (CIMD)
The dataset contains one million Chinese image-text pairs in total, including detailed image captioning and visual question answering.

### Generation Pipeline

* Image source

  We randomly sample images from two opensource datasets [Wanjuan](https://github.com/opendatalab/WanJuan1.0) and [Wukong](https://wukong-dataset.github.io/wukong-dataset/)
* Detailed caption generation

  We use [Gemini Pro Vision API](https://ai.google.dev/) to generate a detailed description for each image. 
* Question-answer pairs generation 

  Based on the generated caption, we use Gemini api to generate a complex question for the corresbonding image. Then, the detailed answer is generated with existing information by Gemini api again.

  For multi-round conversations, the next question is generated with the conversation history.
* Post-processing

  To further improve the quaility of the generated instruction data, we discard the image-text pairs with multiple repetitions in the text. 

  We found some answers generated by Gemini may have hallucination to a certain degree, so we filter the unreasonable generated image-text pairs by asking Gemini again.

### Detailed information of the datasets
| Image Source | #Images | #Captions | #Single-turn QA Pairs | #Multi-turn QA pairs | #Total Image-text Pairs |
| --- | ---: | ---: | ---: | ---: | ---: | 
[Wanjuan](https://github.com/opendatalab/WanJuan1.0)	|212,326|	200,917|	539,371	|65,005|	805,293
[Wukong](https://wukong-dataset.github.io/wukong-dataset/)|	66,570	|106,368|	107,991	|0|	214,359
total|	278,896|	307,285|	647,362|	65,005|	1,019,652


All datasets can be downloaded [here](https://huggingface.co/datasets/jingzi/CIMD).
We also uploaded all the images as `images.zip`, because we have renamed the original image name during downloading for convenience. 


###  Samples
* single-turn conversation with detailed caption (image from wanjuan dataset)

```
{
  "id": "4fdb831b09ab87f3dfa758cbb3693eb2dec41b7a5cb0eeb55a6eb5faae2c7fb1_2",
  "image": "wanjuan/4fdb831b09ab87f3dfa758cbb3693eb2dec41b7a5cb0eeb55a6eb5faae2c7fb1.jpg", 
  "conversations": [
  {
    "from": "human", 
    "value": " 这些屋顶的坡度各不相同，为什么有些屋顶的坡度较大，而有些屋顶的坡度较小呢？\n<image>"
  },
  {
    "from": "gpt", 
    "value": " 屋顶的坡度与当地的气候条件有关。坡度大的屋顶可以更快地排水，更适合于多雨地区。坡度小的屋顶更适合于少雨地区。"
  }
  ], 
  "detailed_caption": " 这是一组中国传统民居的屋顶图片。屋顶由青瓦铺成，屋脊上有各种装饰。屋顶的坡度很大，有利于排水。屋顶上还有许多小老虎，用以辟邪。"
}
```
* multi-turn conversation without detailed caption (image from wanjuan dataset)

```
{
  "id": "787add42091e8f7f2ec23ab48fcbdf2201859fd7dd9c6ba95ae373aa11a7f9c8_0",
  "image": "wanjuan/787add42091e8f7f2ec23ab48fcbdf2201859fd7dd9c6ba95ae373aa11a7f9c8.jpg", 
  "conversations": [
  {
    "from": "human", 
    "value": " 这张图片是一套咖啡店的VI设计，包括了名片、信纸、封套、宣传册。请问这套设计体现了咖啡店的哪些特色？\n<image>"
  }, 
  {
    "from": "gpt",
    "value": " 这套咖啡店的VI设计体现了咖啡店的以下特色：\n\n1. 现代感：这套设计使用了现代感十足的几何图形和线条，给人以时尚、摩登的感觉。\n2. 简约性：这套设计并没有采用过多的装饰元素，而是以简约的风格为主，给人以干净、清爽的感觉。\n3. 自然感：这套设计使用了大量的留白，并且在名片和信纸上都运用了大面积的绿色，给人以自然、清新之感。\n4. 趣味性：这套设计在一些细节之处加入了一些趣味性的元素，比如在名片的背面加入了一个小插图，在信纸的抬头处加入了一个咖啡杯的图案，这些小细节为这套设计增添了些许趣味。"
  },
  {
    "from": "human", 
    "value": " 好的，我根据你的描述，给你设计了一个新的问题：\n\n这套咖啡店的VI设计使用了哪些颜色？这些颜色分别代表了什么？"
  },
  {
    "from": "gpt", "value": " 这套咖啡店的VI设计使用了黄色、绿色和黑色。黄色代表着阳光、活力和热情；绿色代表着自然、清新和健康；黑色代表着沉稳、高贵和典雅。这三种颜色搭配在一起，给人以一种非常舒适的感觉，同时也很好地体现了咖啡店的特色。"
  }
  ],
  "detailed_caption": ""
}
```
* single-turn conversation with detailed caption (image from wukong dataset)

```
{
  "id": "b97b0ec2d43f84d2c424689db9592d26_1", 
  "image": "wukong/b97b0ec2d43f84d2c424689db9592d26.jpg", 
  "conversations": [
  {
    "from": "human", 
    "value": " 为什么熊猫头上会有一个蝴蝶结？\n<image>"
  }, 
  {
    "from": "gpt", 
    "value": " 图片是一张熊猫头的表情包，它头上有一个粉色的蝴蝶结。在现实生活中，熊猫头上一般不会有蝴蝶结。但在图片中，熊猫头上有一个蝴蝶结，这可能是为了装饰，或者是为了搞笑。"
  }
  ], 
  "detailed_caption": " 这张图片是一张熊猫头的表情包，它头上有一个粉色的蝴蝶结，表情看起来很滑稽。图片的文字是“谢谢你啊，这么忙还来亲自骂我”，语气中带着一丝无奈和自嘲。"
}
```
