### overlayWith
将(合成)图像覆盖在经过处理的(调整大小、提取等)图像上。

覆盖图像必须与处理后的图像相同或更小。如果同时提供了`top`和`left`选项，则它们优先于`gravity`。

如果覆盖图像包含一个alpha通道，那么将进行组合和预乘法。

参数
* overlay (Buffer|String) 包含图像数据的缓冲区或包含图像文件路径的字符串。
* options (Object)
    * gravity (String) 放置叠加的位置。(可选,默认`centre`)
    * top (Number) 从上边缘开始的像素偏移量
    * left (Number) 从左边缘开始的像素偏移量。
    * tile (Boolean) 设置为true，以在给定`gravity`下在整个图像上重复覆盖图像。(可选,默认`false`)
    * cutout (Boolean) 设置为true，只对输入图像应用覆盖图像的alpha通道，使一个图像看起来像是从另一个图像中裁剪出来的。(可选,默认`false`)
    * density (Number) 数字表示矢量叠加图像的DPI。(可选,默认72)
    * raw (Object) 使用原始像素数据时的叠加是的描述
        * width
        * height
        * channels
    * create (Object) 创建一个空白的图层的描述
        * width 
        * height 
        * channels
        * background (String|Object) 由[color](https://www.npmjs.org/package/color)模块解析，提取红色、绿色、蓝色和alpha值。

参数不正确抛出异常 正常返回sharp实例

栗子
```js
sharp('input.png')
    .rotate(180)
    .resize(300)
    .flatten()
    .background('#ff6600')
    .overlayWith('overlay.png', { gravity: sharp.gravity.southeast } )
    .sharpen()
    .withMetadata()
    .webp( { quality: 90 } )
    .toBuffer()
    .then(function(outputBuffer) {
        // outputBuffer contains upside down, 300px wide, alpha channel flattened
        // onto orange background, composited with overlay.png with SE gravity,
        // sharpened, with metadata, 90% quality WebP image data. Phew!
    });
```