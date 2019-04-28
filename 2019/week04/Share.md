# Block原理及应用

## 数据结构

在 objc 中，根据对象的定义，凡是首地址是 `*isa` 的结构体指针，都可以认为是对象(id)。这样在objc中，block 实际上就算是对象。

```
struct Block_descriptor_1 {
    uintptr_t reserved;
    uintptr_t size;
};

struct Block_layout {
    void *isa;
    volatile int32_t flags; // contains ref count
    int32_t reserved; 
    void (*invoke)(void *, ...);
    struct Block_descriptor_1 *descriptor;
    // imported variables
};
```

* isa 指针，所有对象都有该指针，用于实现对象相关的功能。
* flags，用于按 bit 位表示一些 block 的附加信息。
* reserved，保留变量。
* invoke，函数指针，指向具体的 block 实现的函数调用地址。
* descriptor， 表示该 block 的附加描述信息，主要是 size 大小，以及 copy 和 dispose 函数的指针。
* variables，capture 过来的变量，block 能够访问它外部的局部变量，就是因为将这些变量（或变量的地址）复制到了结构体中。

## block 的 copy

block中的isa指向的是该block的Class。在 block runtime 中，定义了6种类，能接触的主要是前三种：

1. _NSConcreteStackBlock     栈上创建的block
2. _NSConcreteMallocBlock    堆上创建的block
3. _NSConcreteGlobalBlock    作为全局变量的block
4. _NSConcreteWeakBlockVariable
5. _NSConcreteAutoBlock
6. _NSConcreteFinalizingBlock

block的变化

1. 当struct第一次被创建时，它是存在于该函数的栈帧上的，其Class是固定的_NSConcreteStackBlock。
2. 当函数返回时，函数的栈帧被销毁，这个block的内存也会被清除。
3. 在函数结束后仍然需要这个block时，就必须用Block_copy()方法将它拷贝到堆上。这个方法的核心动作很简单：申请内存，将栈数据复制过去，将Class改一下，最后向捕获到的对象发送retain，增加block的引用计数。

在 ARC 开启的情况下，将只会有 NSConcreteGlobalBlock 和 NSConcreteMallocBlock 类型的 block。

## 对变量的捕获规则

1. 静态存储区的变量：例如全局变量、方法中的static变量。引用，可修改。
2. block接受的参数。传值，可修改，和一般函数的参数相同。
3. 栈变量 (被捕获的上下文变量)。const，不可修改。 当block被copy后，block会对 id类型的变量产生强引用。每次执行block时,捕获到的变量都是最初的值。
4. 栈变量 (有__block前缀)。引用，可以修改。如果时id类型则不会被block retain,必须手动处理其内存管理。如果该类型是C类型变量，block被copy到heap后,该值也会被挪动到heap。

## 使用

### 定义

```
typedef returnType (^TypeName)(parameterTypes);
TypeName blockName = ^returnType(parameters) {...};
```

```
typedef void(^MyBlock)(void);
typedef NSString *(^MyStringBlock)(NSString *args);

MyBlock myBlock = ^(){
    NSLog(@"my block");
};
myBlock();

MyStringBlock myStringBlock = ^(NSString *args){
    return [NSString stringWithFormat:@"in:%@", args];
};
NSLog(@"%@", myStringBlock(@"in"));
```

### 局部变量

```
returnType (^blockName)(parameterTypes) = ^returnType(parameters) {...};
```

```
NSString * (^localBlock)(NSString *args) = ^NSString *(NSString *args){
    return [NSString stringWithFormat:@"result:%@", args];
};
NSLog(@"%@", localBlock(@"in"));
```

### 属性

```
@property (nonatomic, copy, nullability) returnType (^blockName)(parameterTypes);
```

```
// 定义
@property (nonatomic, copy, nullable) NSString *(^propertyBlock)(NSString *args);
@property (nonatomic, copy, nullable) MyStringBlock stringBlock;

// 赋值
self.propertyBlock = ^NSString *(NSString *args) {
        return [NSString stringWithFormat:@"result:%@", args];
    };
NSLog(@"%@", self.propertyBlock(@"property block"));
```

### 方法参数及调用

```
- (void)someMethodThatTakesABlock:(returnType (^nullability)(parameterTypes))blockName;

[someObject someMethodThatTakesABlock:^returnType (parameters) {...}];
```

```
// 定义函数及 block
- (void)blockMethodWithBlock:(NSString *(^)(NSString *args))block
{
    NSString *result = block(@"args");
    NSLog(@"%@", result);
}

// 函数调用及 block 实现
[self blockMethodWithBlock:^NSString *(NSString *args) {
    return [NSString stringWithFormat:@"result:%@", args];
}];
```

## 实例

### 后台运行

当 app 被按 home 键退出后，app 仅有最多 5 秒钟的时候做一些保存或清理资源的工作。但是应用可以调用 UIApplication 的beginBackgroundTaskWithExpirationHandler方法，让 app 最多有 10 分钟的时间在后台长久运行。这个时间可以用来做清理本地缓存，发送统计数据等工作。

```
// AppDelegate.h 文件
@property (assign, nonatomic) UIBackgroundTaskIdentifier backgroundUpdateTask;
// AppDelegate.m 文件
- (void)applicationDidEnterBackground:(UIApplication *)application
{
    [self beingBackgroundUpdateTask];
    // 在这里加上你需要长久运行的代码
    [self endBackgroundUpdateTask];
}
- (void)beingBackgroundUpdateTask
{
    self.backgroundUpdateTask = [[UIApplication sharedApplication] beginBackgroundTaskWithExpirationHandler:^{
        [self endBackgroundUpdateTask];
    }];
}
- (void)endBackgroundUpdateTask
{
    [[UIApplication sharedApplication] endBackgroundTask:self.backgroundUpdateTask];
    self.backgroundUpdateTask = UIBackgroundTaskInvalid;
}
```

## 使用注意事项

1. 值类型，__block，block 块内可对其进行修改
2. 引用类型，__weak，block 可进行引用，不会强引用
3. 成员变量，用一个临时变量指向成员变量，在block中操作这个临时变量 `id tmpIvar = _ivar`

