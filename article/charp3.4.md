### resize

调整图片大小 `width,height` 或 `width  x height`

当提供了`width`和`height`时，图像应符合下列可能的方法:
* cover 裁剪以适合提供的两个维度(默认)。
* contain 嵌入在提供的两个维度中。
* fill 忽略输入的长宽比，向两个提供的维度拉伸。
* inside 保留长宽比，将图像的大小调整到尽可能大，同时确保其尺寸小于或等于指定的尺寸
* outside 保留长宽比，将图像的大小调整到尽可能小，同时确保其尺寸大于或等于指定的尺寸。其中一些值基于[object-fit](https://developer.mozilla.org/en-US/docs/Web/CSS/object-fit) CSS属性。

当用`fit、cover、contai`中的一个值时，默认`postion`值为`centre` ,其他可选值有：
* position : `top, right top, right, right bottom, bottom, left bottom, left, left top`
* gravity : `north, northeast, east, southeast, south, southwest, west, northwest, center or centre`
* strategy : `cover`只有动态地利用`entropy`或`attention`策略进行作物收割。其中一些值基于[object-position](https://developer.mozilla.org/en-US/docs/Web/CSS/object-position) CSS属性

基于实验策略的方法对边缘区域进行了调整，使一维在目标长度处，然后对边缘区域进行重复排序，根据所选策略丢弃得分最低的边缘。

* entropy 关注香农熵最大的区域。参考:[Shannon entropy](https://en.wikipedia.org/wiki/Entropy_%28information_theory%29)
* attention 集中在亮度频率最高的区域，色彩饱和度和肤色的存在。

可能的插值核有:
* nearest 使用最近邻居插值。 [nearest neighbour interpolation](http://en.wikipedia.org/wiki/Nearest-neighbor_interpolation)
* cubic 使用Catmull-Rom样条。 [Catmull-Rom spline](https://en.wikipedia.org/wiki/Centripetal_Catmull%E2%80%93Rom_spline)
* lanczos2 使用a=2的Lanczos内核。[Lanczos kernel](https://en.wikipedia.org/wiki/Lanczos_resampling#Lanczos_kernel)
* lanczos3 使用a=3(默认值)的Lanczos内核。


参数：

* width (Number)调整图像的宽度值。使用`null`或`undefined`自动缩放宽度以匹配高度。
* height (Number) 调整图像的高度值。使用`null`或`undefined`自动缩放高度以匹配宽度。
* options (Object)
    * width (String) 指定`width`的替代方法。如果双方都在场，优先考虑。
    * height (String) 指定`height`的替代方法。如果双方都在场，优先考虑。
    * fit (String) 图像应如何调整大小，以适应提供的尺寸，`cover, contain, fill, inside or outside`中的一个值。(可选,默认`cover`)
    * position (String) 当`fit`属性是`cover`或`contain`时， `position`的使用值 (可选 默认是`centre`)
    * background(String|Object) 当使用`contain`作为`fit`的值时的背景颜色，由颜色模块解析，默认为黑色，没有透明度。(可选,默认{0,g: 0, b: 0,α:1})
    * kernel (String) 用于图像压缩的内核。(可选,默认`lanczos3`)
    * withoutEnlargement (Boolean) 如果宽度或高度已经小于指定的尺寸，则不要放大，这相当于GraphicsMagick的`>`几何选项。(可选,默认`false`)
    * fastShrinkOnLoad (Boolean) 充分利用JPEG和WebP的收缩加载特性，这可以导致一些图像上出现轻微的波纹图案。(可选,默认`true`)

参数不正确抛出异常 正常返回sharp实例

栗子
```js
//eg.1
sharp(input)
  .resize({ width: 100 })
  .toBuffer()
  .then(data => {
    // 100 pixels wide, auto-scaled height
  });

//eg.2
sharp(input)
  .resize({ height: 100 })
  .toBuffer()
  .then(data => {
    // 100 pixels high, auto-scaled width
  });

//eg.3
sharp(input)
  .resize(200, 300, {
    kernel: sharp.kernel.nearest,
    fit: 'contain',
    position: 'right top',
    background: { r: 255, g: 255, b: 255, alpha: 0.5 }
  })
  .toFile('output.png')
  .then(() => {
    // output.png is a 200 pixels wide and 300 pixels high image
    // containing a nearest-neighbour scaled version
    // contained within the north-east corner of a semi-transparent white canvas
  });
// eg.4
const transformer = sharp()
  .resize({
    width: 200,
    height: 200,
    fit: sharp.fit.cover,
    position: sharp.strategy.entropy
  });
// Read image data from readableStream
// Write 200px square auto-cropped image data to writableStream
readableStream
  .pipe(transformer)
  .pipe(writableStream);

//eg.5
sharp(input)
  .resize(200, 200, {
    fit: sharp.fit.inside,
    withoutEnlargement: true
  })
  .toFormat('jpeg')
  .toBuffer()
  .then(function(outputBuffer) {
    // outputBuffer contains JPEG image data
    // no wider and no higher than 200 pixels
    // and no larger than the input image
  });
```

### extend

使用提供的背景颜色扩展/填充图像的边缘。这个操作总是在调整大小和提取(如果有的话)之后进行。

参数：
* extend (String|Object) 单个像素计数，用于添加到所有边缘或具有每个边缘计数的对象
    * top (Number)
    * left (Number)
    * bottom (Number)
    * right (Number)
    * background (String|Object) 背景颜色，由[color](https://www.npmjs.org/package/color)模块解析，默认为黑色，没有透明度。(可选,默认{0,g: 0, b: 0,α:1})

参数不正确抛出异常 正常返回sharp实例

栗子
```js
// Resize to 140 pixels wide, then add 10 transparent pixels
// to the top, left and right edges and 20 to the bottom edge
sharp(input)
  .resize(140)
  .)
  .extend({
    top: 10,
    bottom: 20,
    left: 10,
    right: 10
    background: { r: 0, g: 0, b: 0, alpha: 0 }
  })
```

### extract

提取图像的一个区域。

在`resize`之前使用 获取调整前的提取

在`resize`之后使用 获取调整后提取

之前之后都使用  都获取

参数

* left 从左边缘开始的零索引偏移量
* top zero-indexed offset from top edge
* width 提取图像的宽度
* height 提取图像的高度

参数不正确抛出异常 正常返回sharp实例

栗子
```js
//eg.1
sharp(input)
  .extract({ left: left, top: top, width: width, height: height })
  .toFile(output, function(err) {
    // Extract a region of the input image, saving in the same format.
  });

//eg.2
sharp(input)
  .extract({ left: leftOffsetPre, top: topOffsetPre, width: widthPre, height: heightPre })
  .resize(width, height)
  .extract({ left: leftOffsetPost, top: topOffsetPost, width: widthPost, height: heightPost })
  .toFile(output, function(err) {
    // Extract a region, resize, then extract from the resized image
  });
```

### trim

从所有包含类似左上角像素值的边缘修剪“无效”像素。`info`响应对象将包含`trimOffsetLeft`和`trimOffsetTop`属性。

参数
* threshold (Number) 许与左上角像素的差值，大于零。(可选的,默认10)

参数不正确抛出异常 正常返回sharp实例