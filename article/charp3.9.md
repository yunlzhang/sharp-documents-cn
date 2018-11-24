### cache
获取或在提供选项时设置libvip操作缓存的限制。缓存中的现有条目将在限制发生任何更改后被删除。该方法总是返回缓存统计信息，这对于确定特定任务需要多少工作内存非常有用。

参数
* options (Object|boolean) 带有以下属性的对象，或使用`true使`用默认缓存设置，`false`删除所有缓存(可选，默认为`true`)
    * memory (Number)此缓存使用的最大内存(MB)(可选，默认为50)
    * files (Number) 是要打开的文件的最大数量(可选，默认为20)
    * items (Number) 缓存的最大操作数(可选，默认为100)

返回Object

栗子
```js
const stats = sharp.cache();
sharp.cache( { items: 200 } );
sharp.cache( { files: 0 } );
sharp.cache(false);
```


### concurrency

获取或(当提供并发时)设置libvip应该创建的用于处理每个映像的线程数。默认值是CPU内核的数量。值0将重置为此默认值。

可以并行处理的最大图像数量受libuv的UV_THREADPOOL_SIZE环境变量的限制

这个方法总是返回当前并发性。

参数：
* concurrency (Number)

返回当前并发性。(Number类型)

栗子
```js
const threads = sharp.concurrency(); // 4
sharp.concurrency(2); // 2
sharp.concurrency(0); // 4
```


### counters

提供对内部任务计数器的访问。

* queue是这个模块排队等待libuv从池中提供一个工作线程的任务数量。
* 进程是当前正在处理的调整大小任务的数量。

栗子
```js
const counters = sharp.counters(); // { queue: 2, process: 4 }
```
返回Object类型数据

### simd

得到并设置使用SIMD向量单元指令。需要使用liborc支持编译libvip。

利用CPU的SIMD矢量单元，如Intel SSE和ARM NEON改进`resize,blur,sharpen`操作的性能

参数
* simd (Boolean) 可选，默认为true

返回Boolean类型

栗子
```js
const simd = sharp.simd();
// simd is `true` if the runtime use of liborc is currently enabled

const simd = sharp.simd(false);
// prevent libvips from using liborc at runtime
```