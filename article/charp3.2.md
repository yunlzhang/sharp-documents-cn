## clone

对Sharp实例进行“快照”，返回一个新实例。克隆实例继承父实例的输入。这允许多个输出流和多个处理管道共享一个输入流。

栗子

```js
const pipeline = sharp().rotate();
pipeline.clone().resize(800, 600).pipe(firstWritableStream);
pipeline.clone().extract({ left: 20, top: 20, width: 100, height: 100 }).pipe(secondWritableStream);
readableStream.pipe(pipeline);
// firstWritableStream receives auto-rotated, resized readableStream
// secondWritableStream receives auto-rotated, extracted region of readableStream
```

返回值sharp实例


## metadata

快速访问(未加密的)图像元数据，无需解码任何压缩图像数据。当没有提供回调时，将返回一个promise/A+ promise

callback(err,metadata)

metadata
* format 用于解压图像数据的解码器的名称，例如jpeg、png、webp、gif、svg
* size 图像的总大小(以字节为单位)，仅用于流和缓冲区输入
* width 像素宽(不考虑外置的方向)
* height 像素高(不考虑外置的方向)
* space 颜色空间的名称，例如srgb、rgb、cmyk、lab、b-w…
* channels 频带数如sRGB为3,CMYK为4
* depth 像素深度格式的名称，如uchar, char, ushort, float…
* density 每英寸像素数(DPI)，如果存在
* chromaSubsamplin  包含JPEG色度子采样的字符串，RGB为4:2:0或4:4:4,CMYK为4:2:0:4或4:4:4
* isProgressive 布尔值，指示图像是否使用渐进式扫描进行交错
* hasProfile 指示嵌入的ICC概要文件的存在的布尔值
* hasAlpha 指示alpha透明通道存在的布尔值
* orientation 如果存在，则EXIF方向标头的数值
* exif 如果存在，包含原始EXIF数据的Buffer
* icc 如果存在，包含原始ICC配置文件数据的缓冲区(如果存在)
* iptc 如果存在，包含原始IPTC数据的缓冲区
* xmp 如果存在，包含原始XMP数据的缓冲区

栗子

```js
const image = sharp(inputJpg);
image
  .metadata()
  .then(function(metadata) {
    return image
      .resize(Math.round(metadata.width / 2))
      .webp()
      .toBuffer();
  })
  .then(function(data) {
    // data contains a WebP image half the width and height of the original JPEG
  });
```

返回值 `(Promise<Object> | Sharp)`


## stats

访问图像中每个通道的像素派生的图像统计信息。在没有提供回调时返回一个Promise。

callback(err,stats)

stats

* channels 图像中每个通道的通道统计信息数组。每个通道统计包含:
    * min 通道中的最小值
    * max 通道中的最大值
    * sum 通道中所有值的总和
    * squaressSum 通道中平方值的和
    * mean 通道中值的平均值
    * stdev 通道中值的标准差
    * minX 最小值所在像素点之一的x坐标
    * minY 最小值所在像素点之一的y坐标
    * maxX 最大值所在像素点之一的x坐标
    * maxY 最大值所在像素点之一的y坐标
* isOpaque 根据alpha通道的存在和使用，确定图像是不透明的还是透明的
* entropy 基于直方图的灰度熵估计，丢弃alpha通道(实验)

栗子

```javascript
const image = sharp(inputJpg);
image
  .stats()
  .then(function(stats) {
     // stats contains the channel-wise statistics array and the isOpaque value
  });
```
返回值 `Promise<Object>`


## limitInputPixels

如果输入图像的像素数(宽度_高度)超过此限制，则不要处理。假设输入元数据中包含的图像维度是可信的。默认的限制是2684_2689 (0x3FFF _ 0x3FFF)像素

parameters 

* limit `(Number | Boolean)` 一个整数像素，0或false删除限制，true使用默认限制。

参数错误会抛出异常

返回值 sharp实例


## sequentialRead

将libvips访问方法切换为vips_access_sequence的高级设置。这将减少内存使用，并可以提高某些系统的性能。

函数调用之前的默认行为为false，这意味着libvip访问方法不是连续的。

parameters
* sequentialRead`Boolean` (可选，默认true)