### tint
使用所提供的色度对图像着色，同时保持图像亮度。alpha通道可能会出现，并且不会因操作而改变。

参数
* rgb(String|Object) 由颜色模块解析以提取色度值。

参数不正确抛出异常 正常返回sharp实例

### greyscale

转换为8位灰度;256度灰。这是一个线性运算。如果输入图像是在一个非线性的颜色空间，如sRGB，和`gamma`方法一块使用以获得更好的结果。默认情况下，输出图像将是web友好的sRGB，并包含三个(相同的)颜色通道。这可能被其他锐化操作覆盖，例如`toColourspace('b-w')`，它将生成包含一个颜色通道的输出图像。alpha通道可能会出现，并且不会因操作而改变。

参数
* greyscale (Boolean) 可选值，默认值为`true`

返回sharp实例

### grayscale
greyscale的替代拼写

### toColourspace

设置输出colourspace。默认情况下，输出图像将是web友好的sRGB，其他通道解释为alpha通道。

参数
* colorspace (String) 输出颜色空间 例如`srgb,rgb,cmyk,lab,b-w`等等

参数不正确抛出异常 正常返回sharp实例


### toColorspace
`toColourspace`的替代拼写