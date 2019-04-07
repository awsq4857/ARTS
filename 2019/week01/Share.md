# flutter分析

> flutter做为Google推出的跨平台框架，最近比较流行。在还没有进行大量运用之前，先学习学习，为以后跨平台开发打打基础。

## flutter原理，如何做到跨平台

> 渲染引擎，依赖跨平台的Skia图形库。依赖系统的只有图形绘制相关的接口。

## flutter怎么开发跨平台app

基于flutter的Material Design风格或者Cupertino风格，开发App。flutter会根据不同平台，生成app。

在Android平台，Skia使用FreeType进行渲染；在iOS平台，Skia使用CoreGraphics进行渲染。

一套代码，只能开发一套风格的App。如果App在Android上是Material，在iOS上是Cupertino风格，则需要针对不同平台，编写UI。

## 当引入第三方库时，如何处理

### 1. 通过flutter管理库-pub

flutter使用pub进行仓库管理，类似于Android的Gradle、iOS的CocoaPods。

### 2. 引用原生代码

在Android和iOS原生工程中，引入第三方库。通过dart代码，与原生代码交互，引用第三方库。

## 已有工程，如何进行混合开发

1. Android，打包成aar，集成到现有工程中
2. iOS，打包成framework，集成到现有工程中

## 链接

1. [Flutter原理与美团的实践](https://blog.csdn.net/MeituanTech/article/details/81567238)
2. [深入理解flutter的编译原理与优化](https://yq.aliyun.com/articles/604052?utm_content=m_1000004305)

