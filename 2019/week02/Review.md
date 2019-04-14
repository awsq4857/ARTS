# [一道高级iOS面试题](https://mp.weixin.qq.com/s/N1m6Y3H-wHg5__h4HxCGYA)

通过一道`Runtime`的题目，回顾知识点。

```题目
//MNPerson
@interface MNPerson : NSObject

@property (nonatomic, copy)NSString *name;

- (void)print;

@end

@implementation MNPerson

- (void)print{
    NSLog(@"self.name = %@",self.name);
}

@end

---------------------------------------------------

@implementation ViewController

- (void)viewDidLoad {

    [super viewDidLoad];

    id cls = [MNPerson class];

    void *obj = &cls;

    [(__bridge id)obj print];

}
```

问题

1. 为什么没Crash
2. 为什么输出的是`self.name = <ViewController`

## 知识点

1. 对象创建时，通过类结构体，为对象在堆上分配内存空间。
2. 方法调用，是在对象或类的isa类表中，查找对应的实现。
3. 栈上变量的地址，是逆序排列

## 为什么没Crash

1. `[MNPerson class]`，找到`Person`的类结构体。
2. `&cls`，指向结构体的首地址。
3. 结构体的首地址，是isa。而isa里面，记录有方法列表，从而找到了print方法。
4. 正常我们创建对象，new或者init，是在堆中分配了对应的地址。调用方法时，指向的是分配地址后isa所指向的函数。

## 为什么是`<ViewController`

1. `self.name`，通过`MNPerson_IMPL`结构体，找到name。
2. `MNPerson`，只有两个变量。name其实就是`isa`跳过8个字节。
3. `self.name`，就是先找到`self`的`isa`，再跳8个字节。
4. `[super viewDidLoad]`，底层是`objc_msgSendSuper({ self, [ViewController class] },@selector(ViewDidLoad))`
5. 这里，self就是`ViewController_IMPL `，跳8个字节，就是`[ViewController class]`

```
struct temp = {
    self,
    [ViewController class] 
}

objc_msgSendSuper(temp, @selector(ViewDidLoad))
```