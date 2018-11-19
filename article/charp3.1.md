## sharp

> 备注：以下所有number类型的参数必须是整型

sharp(input | options | 无参数);

* input`Buffer | String` 可以是包含JPEG、PNG、WebP、GIF、SVG、TIFF或原始像素图像数据的Buffer，或包含JPEG、PNG、WebP、GIF、SVG或TIFF图像文件路径的字符串。

* options`Object` 是具有可选属性的对象。
    * failOnError`Boolean` 默认情况下，应用“最佳能力”来解码图像，即使数据损坏或无效。如果您宁愿停止处理并在加载无效图像时引发错误，则将此标志设置为true。(可选,默认false)
    * density`Number` 向量图像的DPI的数字。 (可选，默认：72)
    * page`Number` 提取多页输入的页码(GIF、TIFF) (可选，默认0)
    * raw`Object` 描述原始像素输入图像数据。有关像素排序，请参见`raw()`。
        * width`Number`
        * height`Number`
        * channels`Number`  1-4
    * create `Object` 描述要创建的新图像。
        * width`Number`
        * height `Number`
        * channels `Number` 1-4
        * background `String | Object` 由颜色模块解析以提取红色、绿色、蓝色和alpha值。

栗子：

```javascript
    // 1
    var sharp1 = sharp('input.jpg')

    //2
    var sharp2 = sharp({
        create: {
            width: 300,
            height: 200,
            channels: 4,
            background: { r: 255, g: 0, b: 0, alpha: 0.5 }
        }
    })
```

无效参数时抛出错误

返回值 sharp实例


## format

包含表示可用输入和输出格式/方法的嵌套布尔值的对象

栗子：

```js
console.log(sharp.format);
```
返回值 Object


## versions 

包含libvip版本号及其依赖项的对象。

栗子：
```js
console.log(sharp.versions);
```


## queue

一个EventEmitter，当一个任务有如下情形时发出通知：
* 排队，等待libuv提供一个工作线程
* 完成

栗子：

```js
sharp.queue.on('change', function(queueLength) {
    console.log('Queue contains ' + queueLength + ' task(s)');
});
```

返回值 Object
