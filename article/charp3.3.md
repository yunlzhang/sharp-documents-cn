### toFile
将输出图像数据写入文件。

如果没有选择输出格式，将从扩展中推断它，支持JPEG、PNG、WebP、TIFF、DZI和libvip的V格式。注意，原始像素数据只支持缓冲区输出。

未提供回调时，将返回`Promise`。

参数

* fileOut (String) 写入图像数据的路径
* callback (Function) 在完成时调用，带有两个参数(err, info)。info包含输出图像格式、大小(字节)、宽度、高度、通道和预乘(指示是否使用预乘)。在使用裁剪策略时还包含cropOffsetLeft和cropOffsetTop。

栗子

```js
sharp(input).toFile('output.png', (err, info) => { ... });

sharp(input)
  .toFile('output.png')
  .then(info => { ... })
  .catch(err => { ... });
```


### toBuffer

将输出写入缓冲区。支持JPEG、PNG、WebP、TIFF和RAW输出。默认情况下，格式将匹配输入图像，除了GIF和SVG输入成为PNG输出。

回调，如果存在，得到三个参数(err, data, info)，其中:
* err 一个错误(如果有的话)。
* data 输出的图像数据
* info 包含输出图像格式、大小(字节)、宽度、高度、通道和预乘(指示是否使用预乘)。在使用裁剪策略时还包含cropOffsetLeft和cropOffsetTop。

未提供回调时，将返回`Promise`。

参数
* options 
    * resolveWithObject 使用包含`data`和`info`属性的对象解析`Promise`，而不是仅使用`data`解析。
*callback 回调

栗子
```js
//eg.1
sharp(input)
  .toBuffer((err, data, info) => { ... });

//eg.2
sharp(input)
  .toBuffer()
  .then(data => { ... })
  .catch(err => { ... });

//eg.3
sharp(input)
  .toBuffer({ resolveWithObject: true })
  .then(({ data, info }) => { ... })
  .catch(err => { ... });

```


### withMetadata

在输出图像中包含来自输入图像的所有元数据(EXIF、XMP、IPTC)。不使用元数据时，默认行为是剥离所有元数据并转换为独立于设备的sRGB颜色空间。这也将转换和添加一个web友好的sRGB ICC配置文件。

参数
* withMetadata
    * orientation (Number) 介于1和8之间，用于更新EXIF方向标记。

参数不正确抛出异常 正常返回sharp实例

栗子
```js
sharp('input.jpg')
  .withMetadata()
  .toFile('output-with-metadata.jpg')
  .then(info => { ... });

```

### jpeg

使用JPEG格式输出图像。

参数
* options(Object)
    * quality (Number) 图片质量，整数1-100(可选，默认80)
    * progressive (Boolean) 使用渐进式(交错)扫描(可选，默认为false)
    * chromaSubsampling (String) 设置为“4:4:4”，以防止质量<= 90时色度子采样(可选，默认为“4:2:0”)
    * trellisQuantisation (Boolean) 应用网格量化，需要mozjpeg(可选，默认为false)
    * overshootDeringing (Boolean) 应用超调脱靶，需要mozjpeg(可选，默认为false)
    * optimiseScans (Boolean) 优化渐进式扫描，强制渐进式扫描，要求mozjpeg(可选，默认为false)
    * optimizeScans (Boolean) optimisescan的替代拼写(可选，默认为false)
    * optimiseCoding (Boolean) 优化Huffman编码表(可选，默认为true)
    * optimizeCoding (Boolean) optimiseCoding的替代拼写(可选，默认为true)
    * quantisationTable (Number) 要使用量子化表，整数0-8，需要mozjpeg(可选，默认为0)
    * quantizationTable(Number) quantisationTable的替代边写，整数0-8，需要mozjpeg(可选，默认为0)
    * force (Boolean) 强制JPEG输出，否则尝试使用输入格式(可选，默认为true)

参数不正确抛出异常 正常返回sharp实例

栗子
```js
// Convert any input to very high quality JPEG output
const data = await sharp(input)
  .jpeg({
    quality: 100,
    chromaSubsampling: '4:4:4'
  })
  .toBuffer();
```

### png

使用png格式输出图片

PNG输出总是全色的，每像素8或16位。每像素1、2或4位的索引PNG输入被转换为每像素8位。

参数
* options (Object)
    * progressive (Boolean) 使用渐进式(交错)扫描(可选，默认为false)
    * compressionLevel (Number) zlib压缩级别，0-9(可选，默认9)
    * adaptiveFiltering (Boolean) 使用自适应行筛选(可选，默认为false)
    * force (Boolean) 强制PNG输出，否则尝试使用输入格式(可选，默认为true)

参数不正确抛出异常 正常返回sharp实例

栗子
```js
// Convert any input to PNG output
const data = await sharp(input)
  .png()
  .toBuffer();

```

### webp

使用webp格式输出图片

参数
* options (Object)
    * quality (Number) 质量，整数1-100(可选，默认80)
    * alphaQuality (Number) alpha层的质量，整数0-100(可选，默认100)
    * lossless (Boolean) 使用无损压缩模式(可选，默认为false)
    * nearLossless (Boolean) 使用接近无损压缩模式(可选，默认为false)
    * force (Boolean) 强制WebP输出，否则尝试使用输入格式(可选，默认为true)

参数不正确抛出异常 正常返回sharp实例

栗子
```js
// Convert any input to lossless WebP output
const data = await sharp(input)
  .webp({ lossless: true })
  .toBuffer();
```


### tiff

使用tiff格式输出图像

参数
* options(Object)
    * quality (Number) 质量，整数1-100(可选，默认80)
    * force (Boolean) 强制TIFF输出，否则尝试使用输入格式(可选，默认为true)
    * compression (Boolean) 压缩选项:lzw, deflate, jpeg, ccittfax4(可选，默认'jpeg')
    * predictor (String) 压缩预测器选项:无、水平、浮动(可选、默认“水平”)
    * xres (Number) 水平分辨率(像素/mm)(可选，默认1.0)
    * yres (Number) 垂直分辨率(像素/mm)(可选，默认1.0)
    * squash (Boolean) 将8位图像压缩到1位(可选，默认为false)

参数不正确抛出异常 正常返回sharp实例

栗子
```js
// Convert SVG input to LZW-compressed, 1 bit per pixel TIFF output
sharp('input.svg')
  .tiff({
    compression: 'lzw',
    squash: true
  })
  .toFile('1-bpp-output.tiff')
  .then(info => { ... });
```


### raw 
强制输出为原始的、未压缩的uint8像素数据。

返回sharp实例

栗子
```js
// Extract raw RGB pixel data from JPEG input
const { data, info } = await sharp('input.jpg')
  .raw()
  .toBuffer({ resolveWithObject: true });
```


### toFormat

强制输出到给定的格式。

参数
* format (String|Object) 作为字符串或具有“id”属性的对象
* options (Object) 输出参数选项 

不支持的格式或选项不合法输出异常

栗子
```js
// Convert any input to PNG output
const data = await sharp(input)
  .toFormat('png')
  .toBuffer();
```


### tile

使用tile-based的深度压缩方式(图像金字塔)输出。通过toFormat、jpeg、png或webp函数设置平铺图像的格式和选项。使用带toFile的.zip或.szi文件扩展名将文件写入压缩归档文件格式。

警告:多个并发生成tile输出的sharp实例可能会在libgsf的某些版本中暴露可能的竞态条件。

参数
* options (Object)
    * size (Number) 瓦片大小(以像素为单位)，值在1到8192之间。(可选,默认256)
    * overlap (Number) 平铺重叠的像素，值介于0和8192之间。(可选,默认0)
    * angle (Number) 瓦片的转角，必须是90的倍数。(可选,默认0)
    * depth (String) 金字塔的深度，可能的值是一个`onepixel`，一个`onetile`或`one`，默认基于布局。
    * container (String) 平铺容器，用`fs`(文件系统)或`zip`(压缩文件)。(可选,默认`fs`)
    * layout (String) 文件系统布局，可能的值是`dz`, `zoomify`或`google`。(可选,默认`dz`)

不支持的格式或参数不合法输出异常

栗子
```js
sharp('input.tiff')
  .png()
  .tile({
    size: 512
  })
  .toFile('output.dz', function(err, info) {
    // output.dzi is the Deep Zoom XML definition
    // output_files contains 512x512 tiles grouped by zoom level
  });
```