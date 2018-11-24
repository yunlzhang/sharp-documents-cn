# sharp

一个利用高速node.js将普通大图片转换成更小的、对web更友好的JPEG、PNG、WebP等不同尺寸的图像的出色的模块。

调整图像大小通常比使用最快的ImageMagick和GraphicsMagick设置还要快4到5倍。

颜色空间、嵌入式ICC配置文件和alpha透明通道都被正确处理。Lanczos重采样确保质量不会为速度而牺牲。

除了图像调整，旋转，提取，合成和gamma校正等操作也是可用的。

大多数运行着node(node版本：6、8、10)的64位OS X、Windows和Linux系统都不需要额外的安装或运行依赖。

### 格式

这个模块支持读取JPEG、PNG、WebP、TIFF、GIF和SVG格式图片 

输出图像可以是JPEG、PNG、WebP和TIFF格式以及未压缩的原始像素数据。

流、缓冲区对象和文件系统可用于输入和输出。

一个输入流可以分成多个处理管道和输出流。

可以生成深度变焦图像金字塔，适合与“滑动地图”瓦片查看器(eg:[OpenSeadragon](https://github.com/openseadragon/openseadragon)和[Leaflet](https://github.com/turban/Leaflet.Zoomify))一起使用

### 快速

这个模块由[libvip](https://github.com/libvips/libvips)图像处理库提供支持，这个库最初是1989年在Birkbeck学院创建的，目前由[John Cupitt](https://github.com/jcupitt)维护。

只有一小部分未压缩的图像数据保存在内存中，并一次处理一次，充分利用多CPU内核和L1/L2/L3缓存。

由于libuv，所有内容都保持非阻塞，没有生成子进程，并且支持promise/async/await。

### 效果佳

Huffman表在生成JPEG输出图像时进行了优化，而无需使用单独的命令行工具，如[jpegoptim](https://github.com/tjko/jpegoptim)和[jpegtran](http://jpegclub.org/jpegtran/)。

默认情况下，PNG滤镜是被禁用的，这对于图表和线条艺术通常产生与[pngcrush](https://pmt.sourceforge.io/pngcrush/)相同的结果。

### 贡献

[贡献者指南](https://github.com/lovell/sharp/blob/master/CONTRIBUTING.md)涵盖了报告bug、请求特性和提交代码更改的指导。


### 贡献人员

如果没有以下人员的帮助和代码贡献，这个模块就不可能实现:

* [John Cupitt](https://github.com/jcupitt)
* [Pierre Inglebert](https://github.com/pierreinglebert)
* [Jonathan Ong](https://github.com/jonathanong)
* [Chanon Sajjamanochai](https://github.com/chanon)
* [Juliano Julio](https://github.com/julianojulio)
* [Daniel Gasienica](https://github.com/gasi)
* [Julian Walker](https://github.com/julianwa)
* [Amit Pitaru](https://github.com/apitaru)
* [Brandon Aaron](https://github.com/brandonaaron)
* [Andreas Lind](https://github.com/papandreou)
* [Maurus Cuelenaere](https://github.com/mcuelenaere)
* [Linus Unnebäck](https://github.com/LinusU)
* [Victor Mateevitsi](https://github.com/mvictoras)

更多请见：[sharp credicts](http://sharp.pixelplumbing.com/en/stable/#credits)

感谢！

### 许可

Copyright 2013, 2014, 2015, 2016, 2017, 2018 Lovell Fuller and contributors.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.



