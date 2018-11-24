### removeAlpha

移除alpha通道，如果有的话。如果图像没有alpha通道，这是一个no-op。

栗子
```js
sharp('rgba.png')
  .removeAlpha()
  .toFile('rgb.png', function(err, info) {
    // rgb.png is a 3 channel image without an alpha channel
  });
```

返回sharp实例

### extractChannel
从多通道图像中提取单个通道

参数
* channel (Number|String) 以零索引带号提取，或以`reg,green,blue`分别替代`0,1,2`。

参数不正确抛出异常 正常返回sharp实例

栗子
```js
sharp(input)
  .extractChannel('green')
  .toFile('input_green.jpg', function(err, info) {
    // info.channels === 1
    // input_green.jpg contains the green channel of the input image
   });
```

### joinChannel
将一个或多个通道连接到图像。添加通道的含义取决于输出colourspace，设置为toColourspace()。默认情况下，输出图像将是web友好的sRGB，其他通道解释为alpha通道。通道顺序遵循vip约定:
* sRGB: 0: Red, 1: Green, 2: Blue, 3: Alpha.
* CMYK: 0: Magenta, 1: Cyan, 2: Yellow, 3: Black, 4: Alpha.

缓冲区可以是夏普支持的任何图像格式:JPEG、PNG、WebP、GIF、SVG、TIFF或原始像素图像数据。对于原始像素输入，options对象应该包含一个原始属性，该属性遵循sharp()构造函数中同名属性的格式。

参数
* images `(Array<(String | Buffer)> | String | Buffer)` 一个或多个图片(文件路径、缓冲区)。
* options (Object) 参考`sharp`构造器

参数不正确抛出异常 正常返回sharp实例


### bandbool
对所有输入图像通道(带)执行逐位布尔运算，生成单个通道输出图像。

参数：
* boolOp  `and,or,eor`中的一个

栗子
```js
sharp('3-channel-rgb-input.png')
  .bandbool(sharp.bool.and)
  .toFile('1-channel-output.png', function (err, info) {
    // The output will be a single channel image where each pixel `P = R & G & B`.
    // If `I(1,1) = [247, 170, 14] = [0b11110111, 0b10101010, 0b00001111]`
    // then `O(1,1) = 0b11110111 & 0b10101010 & 0b00001111 = 0b00000010 = 2`.
  });
```

参数不正确抛出异常 正常返回sharp实例
