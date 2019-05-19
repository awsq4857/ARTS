# [armv6, armv7, armv7s, arm64](http://www.cnblogs.com/lxd2502/p/6140629.html)

> armv6, armv7, armv7s, arm64指的是arm处理器的指令集。
> 
> i386, x86_64指的是pc端处理器指令集。
> 

真机32位处理器需要armv7,或者armv7s架构，真机64位处理器需要arm64架构。

* arm64：iPhone6s | iphone6s plus｜iPhone6｜ iPhone6 plus｜iPhone5S | iPad Air｜ iPad mini2(iPad mini with Retina Display)
* armv7s：iPhone5｜iPhone5C｜iPad4(iPad with Retina Display)
* armv7：iPhone4｜iPhone4S｜iPad｜iPad2｜iPad3(The New iPad)｜iPad mini｜iPod Touch 3G｜iPod Touch4

模拟器32位处理器测试需要i386架构，模拟器64位处理器测试需要x86_64架构。

* i386是针对intel通用微处理器32位处理器
* x86_64是针对x86架构的64位处理器

xcode中，指令集相关的选项主要有三种：

1. Architectures：该选项指定工程可被编译成支持何种指令集的数据包。由于工程会针对每一种指令集编译出对应的二进制数据包，所以支持的指令集越多，对应生成的ipa包就越大。
2. Valid Architectures：限制了工程可支持的指令集范围。即，工程最终支持的指令集在valid architectures定义的这个范围之内，所以工程最终编译出的包支持的指令集将由Architetures和Valid Architectures选项定义的指令集的交集决定。
3. Build Active Architecture Only：设定是否只编译出当前连接设备所支持的指令集。一般，debug的时候可指定为YES，为了debug的时候编译速度快；release的时候指定为NO，以适应不同的设备。

