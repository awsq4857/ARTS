# iOS内存优化

## 内存类型

iOS App的三种内存类型

1. Clean Memory
2. Dirty Memory
3. Compressed Memory

### Clean Memory

> Clean Memory：当内存不足的时候，系统会按照一定策略来腾出更多空间供使用，比较常见的做法是将一部分低优先级的数据挪到磁盘上。

* Code
* frameworks。每个 frameworks 都有 _DATA_CONST 段，当 App 在运行时使用到了某个 framework，它所对应的 _DATA_CONST 的内存就会由 Clean 变为 Dirty
* memory-mapped files。被加载到内存中的文件。

### Dirty Memory

> Dirty Memory：是指那些被 App 写入过数据的内存。

* Heap allocations （所有堆区的对象）
* 图像解码缓冲区
* database caches （ 我们对数据进行缓存的目的是想减少 CPU 的压力，但是过多的缓存又会占用过大的内存。由于内存压缩机制的存在，我们需要根据缓存数据大小以及重算这些数据的成本，在 CPU 和内存之间进行权衡。
在一些需要缓存数据的场景下，可以考虑使用 NSCache 代替 NSDictionary，因为 NSCache 可以自动清理内存，在内存吃紧的时候会更加合理。
）

### Compressed Memory

当内存吃紧的时候，系统会将不使用的内存进行压缩，直到下一次访问的时候进行解压。

例如，当我们使用 Dictionary 去缓存数据的时候，假设现在已经使用了 3 页内存，当不访问的时候可能会被压缩为 1 页，再次使用到时候又会解压成 3 页。

## 工具监控内存

1. Xcode Debug Area
2. Instruments
3. DebugMemoryGraph

## 线上检查工具

Allocations
  
  * FBAllocationTracker

Leaked memory

  * MLeaksFinder
  * FBRetainCycleDetector

## 优化技巧方向

* 视图层级很多的情况做一些处理
* 对一些大的数据或者资源的处理
* 对很多对象的处理
* 避免内存抖动太大，比如可以用@autoreleasepool 处理for循环大量临时对象造成的问题
* 内存泄露的处理
* 收到内存警告时候做一些处理，比如用 NSCache 代替 NSDictionary，使用 NSPurgableData 代替NSData。让系统在内存不足情况下自己清理内存。
* 对图片的处理，比如格式的选择，或者缩放等。

参考链接

1. [iOS性能优化之内存优化](https://mp.weixin.qq.com/s/kzmfc7Y-7jSnHFPKi1ZOog)
2. [iOS Memory Deep Dive](https://mp.weixin.qq.com/s/WQ7rrTJm-cn3Cb6e_zZ4cA)
3. [iOS微信内存监控](https://wetest.qq.com/lab/view/367.html?from=content_juejin)
4. [iOS-Performance-Optimization](https://github.com/skyming/iOS-Performance-Optimization)
5. [WWDC 2018：iOS 内存深入研究](https://juejin.im/post/5b23dafee51d4558e03cbf4f)





