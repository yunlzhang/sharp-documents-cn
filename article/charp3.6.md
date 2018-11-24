### rotate
旋转输出图像的显式角度或自动定向的基础上EXIF的`Orientation`标签。

如果提供了一个角度，则将其转换为有效的正旋转度。例如，-450会产生270度的旋转。

当旋转角度不是90的倍数时，背景颜色可以提供`background`选项。

如果没有提供角度，则由EXIF数据确定。支持镜像，可以推断翻转操作的使用。

使用`rotate`意味着删除EXIF`Orientation`标记(如果有的话)。

方法顺序在旋转和提取区域时都很重要，例如，`rotate(x).extract(y)`与`extract(y).rotate(x)`会产生不同的结果。

参数
* angle (Number) 旋转的角度。(可选,默认`auto`)
* otions (Object)如果存在，则为具有可选属性的对象。
    * background (String|Object) 由[color](https://www.npmjs.org/package/color)(模块解析，提取红色、绿色、蓝色和alpha值。 可选，默认值为你`#000000`

参数不正确抛出异常 正常返回sharp实例

栗子
```js
const pipeline = sharp()
  .rotate()
  .resize(null, 200)
  .toBuffer(function (err, outputBuffer, info) {
    // outputBuffer contains 200px high JPEG image data,
    // auto-rotated using EXIF Orientation tag
    // info.width and info.height contain the dimensions of the resized image
  });
readableStream.pipe(pipeline);
```


### flip 

将图像沿垂直Y轴翻转。这总是在旋转之后发生，如果有的话。flip的使用意味着去掉EXIF`Orientation`标签(如果有的话)。

参数
* flip (Boolean) 可选，默认值是`true`

返回sharp实例


### flop

将图像沿水平X轴翻转。这总是在旋转之后发生，如果有的话。触发器的使用意味着删除EXIF`Orientation`标记(如果有的话)。

参数
* flop (Boolean) 可选，默认值是`true`

返回sharp实例


### sharpen 

锐化图像。当使用无参数，执行快速，温和的锐化输出图像。当提供`sigma`时，在实验室色彩空间中对L通道进行更慢、更精确的锐化。可以单独控制“平”和“锯齿”区域的锐化水平。

参数

* sigama (Number)  高斯模糊`sigama`参数值  `sigma = 1 + radius / 2`
* flat (Number) 锐化的水平适用于“平坦”地区。(可选,默认1.0)
* jagged (Number) 锐化水平适用于“锯齿状”区域。(可选,默认2.0)

参数不正确抛出异常 正常返回sharp实例


### median

运用中值滤波。在没有参数的情况下，默认窗口是3x3。

参数
* size (Number) 正方形掩码大小:x大小(可选，默认3)

参数不正确抛出异常 正常返回sharp实例

### blur

模糊图像。当使用无参数，执行快速，温和的模糊输出图像。当提供`sigma`时，执行更慢、更准确的高斯模糊。

参数
* sigma 0.3到1000之间的值表示高斯掩码的 `sigma = 1 + radius / 2`

参数不正确抛出异常 正常返回sharp实例


### flatten

合并alpha透明通道(如果有的话)和背景。

返回sharp 实例


### gamma

通过将编码(变暗)预调整为`1/gamma`，然后将编码(变亮)后调整为`1/gamma`，应用伽马校正。这可以提高非线性色彩空间中被缩放图像的感知亮度。当应用伽马校正时，JPEG和WebP输入图像不会利用加载收缩性能优化。

参数
* gamma 值介于1.0和3.0之间。(可选,默认2.2)


参数不正确抛出异常 正常返回sharp实例


### negate

产生负片。

参数
* negate (Boolean) 可选，默认是`true`

返回sharp实例


### normalise
通过拉伸亮度来覆盖整个动态范围来增强输出图像的对比度。

参数
* normalise (Boolean) 可选，默认是`true`

返回sharp实例


### normalize

normalise的替代拼写。


### convolve

将图像与指定的内核卷积。

参数
* kernel
    * width  内核的宽度(以像素为单位)。
    * height 内核的高度(以像素为单位)。
    * kernel 内核值包含`width*height`的数组
    * scale  内核的大小，以像素为单位。(可选,默认的`sum`)
    * offset 内核的偏移量，以像素为单位。(可选,默认0)

参数不正确抛出异常 正常返回sharp实例

栗子
```js
sharp(input)
  .convolve({
    width: 3,
    height: 3,
    kernel: [-1, 0, 1, -2, 0, 2, -1, 0, 1]
  })
  .raw()
  .toBuffer(function(err, data, info) {
    // data contains the raw pixel data representing the convolution
    // of the input image with the horizontal Sobel operator
  });
```


### threshold

任何大于或等于阈值的像素值将被设置为255，否则将被设置为0。

参数
* threshold （Number) 0-255范围内的值，表示应用阈值的级别。(可选,默认128)
* options
    * greyscale (Boolean) 转换为单通道灰度。(可选,默认`true`)
    * grayscale greyscale 的替代拼写

参数不正确抛出异常 正常返回sharp实例


### boolean

对操作数映像执行位布尔操作

该操作创建一个输出图像，其中每个像素是在输入图像的相应像素之间选择的逐位布尔操作`operate`的结果。

参数
* operand (Buffer | String) 包含图像数据的缓冲区或包含图像文件路径的字符串。
* operator (String) `and、or、eor` 中的一个值，像C语言逻辑操作符&,|,^。
* options
    * raw 描述在使用原始像素数据时的操作数。
        * width
        * height 
        * channels 

参数不正确抛出异常 正常返回sharp实例


### linear

对图像应用线性公式a * input + b (level adjustment)

参数
* a 乘数(可选，默认1.0)
* b 偏移量(可选，默认0.0)

参数不正确抛出异常 正常返回sharp实例
