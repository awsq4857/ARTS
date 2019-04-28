# [iOS开发 单例使用问题](https://mp.weixin.qq.com/s/TyKbtTwNHlznjCYCw8Eb9g)

## 什么是单例

> `单例模式`：保证一个类仅有一个实例，并提供一个访问它的全局访问点。

```
+(instancetype)sharedInstance
{
    static dispatch_once_t once;
    static id sharedInstance;
    dispatch_once(&once, ^{
        sharedInstance = [[self alloc]init];
    });
    return sharedInstance;
}
```

## 单例中的问题

### 全局状态

单例相对于全局变量，在程序代码的任意地方，都可以直接引用。这样就会造成，单例中的私有变量被修改了，很难追踪到问题。因为可修改的地方多。

### 对象生命周期

单例在整个App生命周期中，基本上是不释放的。这就导致，有时业务场景，会需要释放单例的情况。在这种情况下，尽量不要使用单例。

### 不利于测试

单例一直在整个app的生命周期中存活着，甚至在执行测试的时候也一直存活着，这导致了在一个测试或许会影响另一个测试，这是在单元测试中的大忌。

