# [给 iOS 开发者的 Flutter 指南](https://mp.weixin.qq.com/s/PnLVvOuP7eDa-EjyXAvgdw)

[一](https://mp.weixin.qq.com/s/PnLVvOuP7eDa-EjyXAvgdw)、[二](https://mp.weixin.qq.com/s/59w9e3pdnT5-GqF98J0gYQ)

对于 iOS 开发者来说，是很好的入门资料。通过对比学习，可以很快入门。

## 1. 视图

> 1.1 UIView 相当于 Flutter 中的什么？

在 Flutter 中，Widget 可以类比为 UIView。

* widget 拥有着不同的生命周期： 整个生命周期内它是不可变的，且只能够存活到被修改的时候。
* Flutter 的 widget 是很轻量的，一部分原因就是由于它的不可变特性。因为它并不是视图，也不直接绘制任何内容，而是作为对 UI 及其特性的一种描述，而被“注入”到视图中去。

> 1.2 我该如何更新 Widgets？

StatefulWidget 中有一个State 对象，它用来存储一些状态的信息，并在整个生命周期内保持不变。

> 1.3 如何对 widget 布局？ Storyboard 在哪？

在 Flutter 中，你要通过代码来对 widget 进行 组织来形成一个 widget 树状结构。

```
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text("Sample App"),
    ),
    body: Center(
      child: CupertinoButton(
        onPressed: () {
          setState(() { _pressedCount += 1; });
        },
        child: Text('Hello'),
        padding: EdgeInsets.only(left: 10.0, right: 10.0),
      ),
    ),
  );
}
```

> 1.4 如何添加或移除一个组件？

Flutter 中，因为 widget 是不可变的，所以没有提供直接同 addSubview() 作用相同的方法。但是你可以通过向父视图传递一个返回值是 widget 的方法，并通过一个 boolean flag 来控制子视图的存在。

> 1.5 如何添加动画？

在 Flutter 里，通过使用动画库将 widget 封装到 animated widget 中来实现带动画效果。AnimationController 是一个可以暂停、寻找、停止、反转动画的 Animation<double> 类型。它需要一个 Ticker，在屏幕刷新时发出信号量，并在运行时对每一帧都产生一个 0~1 的线性差值。你可以创建一个或多个 Animation，并把它们添加到控制器中。

> 1.6 如何渲染到屏幕上？

Flutter 里有一套基于 Canvas 实现的 API，有两个类可以帮助你进行绘制：CustomPaint 和 CustomPainter，后者实现了绘制图形到 canvas 的算法。

> 1.7 如何设置视图 Widget 的透明度？

在 iOS 里，视图都有一个 opacity 或者 alpha 属性。而在 Flutter 里，大部分时候你都需要封装 widget 到一个 Opacity widget 中来实现这一功能。

> 1.8 如何构建自定义 widgets？

如果你要构建一个 CustomButton，并在构造器中传入它的文本标签？那就组合 RaisedButton 和文本标签，而不是继承 RaisedButton。 

## 2. 导航

> 2.1 如何在不同页面之间切换？

在Flutter 中，使用 Navigator 和 Routes 也可以实现类似的功能。一个 Routes 是应用中屏幕或者页面的抽象概念，而一个 Navigator 是管多个 Route 的 widget。

`Navigator.of(context).pushNamed('/b');`

获取页面的返回值，使用`await`

```
Map coordinates = await Navigator.of(context).pushNamed('/location');

Navigator.of(context).pop({"lat":43.821757,"long":-79.226392});
```


> 2.2 如何跳转到其他应用？

在 Flutter 里想要实现这个功能，需要创建原生平台的整合层，或者使用已经存在的插件，例如 url_launcher。

> 2.3 如何退回到 iOS 原生的 viewcontroller？

在 Dart 代码中调用 SystemNavigator.pop() 将会调用下面的 iOS 代码：

```
UIViewController* viewController = [UIApplication sharedApplication].keyWindow.rootViewController;
if ([viewController isKindOfClass:[UINavigationController class]]) {
    [((UINavigationController*)viewController) popViewControllerAnimated:NO];
}
```

## 3. 线程和异步

> 3.1 如何编写异步代码？

使用 async / await 来执行网络代码以避免 UI 挂起，让 Dart 来完成这个繁重的任务。

> 3.2 如何让你的工作在后台线程执行？

1. async / await
2. Isolate

> 3.3 如何发起网络请求？

在 async 方法 http.get() 中调用 await

> 3.4 如何展示耗时任务的进度？

在 Flutter 中，应该使用 ProgressIndicator。它在渲染时通过一个 boolean flag 来控制是否显示进度。在耗时任务开始前，告诉 Flutter 去更新状态，并在任务结束后隐藏。 

## 4. 工程结构、本地化、依赖和资源

> 4.1 如何在 Flutter 中引入 图片资源？如何处理多分辨率？

引入图片资源

1. 把一个 JSON 文件放置到 my-assets 文件夹中。`my-assets/data.json`
2. 在 pubspec.yaml 中声明 assets。
3. 在代码中通过使用 AssetBundle 访问资源。

> 4.2 字符串存储在哪里？如何处理本地化？

如果你需要添加其他语言支持，请引入 flutter_localizations 库。 同时你可能还需要添加 intl 库来使用 i10n 机制，比如 日期/时间的格式化等。
 
> 4.3 Cocoapods 相当于 Flutter 中的什么？该如何添加依赖？

在 Flutter 中使用 pubspec.yaml 来声明外部依赖。你可以通过 Pub 来查找一些优秀的 Flutter 第三方包。

## 5. ViewControllers

> 5.1 ViewController 相当于 Flutter 中的什么？

 Flutter 中的屏幕也是使用 Widgets 表示的，因为“万物皆 widget！”。使用 Naivgator 在不同的 Route 之间切换，而不同的路由则代表了不同的屏幕或页面，或是不同的状态，也可能是渲染相同的数据。
 
> 5.2 如何监听 iOS 中的生命周期？

可监听的生命周期事件有：

* inactive - 应用当前处于不活跃状态，不接收用户输入事件。这个事件只在 iOS  上有效，Android 中没有类似的状态。
* paused - 应用处于用户不可见状态，不接收用户输入事件，但仍在后台运行。
* resumed - 应用可见，也响应用户输入。
* suspending - 应用被挂起，在 iOS 平台没有这一事件。

## 6. 布局

> 6.1 UITableView 和 UICollectionView 相当于 Flutter 中的什么？

在 Flutter 里，你可以使用 ListView 来达到类似的实现。由于 Flutter 中 widget 的不可变特性，你需要向 ListView 传递一个 widget 列表，Flutter 会确保滚动快速而流畅。

> 6.2 如何确定列表中被点击的元素？

通过 widget 传递进来的 touch 响应处理来实现。

> 6.3 如何动态更新？

使用 ListView.Builder 来构建列表。

> 6.4 ScrollView 相当于 Flutter 里的什么？

在 Flutter 中，使用 ListView widget 是最简单的办法。它和 iOS 中 ScrollView  及 TableView 表现一致，也可以给它的 widget 做垂直排版。

## 7. 手势检测与 touch 事件处理

> 7.1 如何给 Flutter 的 widget 添加点击事件？

1. 如果 widget 本身支持事件检测，则直接传递处理函数给它。例如，RaisedButton 拥有 一个 onPressed 参数
2. 如果 widget 本身不支持事件检测，那么把它封装到一个 GestureDetector 中，并给它的 onTap 参数传递一个函数。

> 7.2 我怎么处理 widget 上的其他手势？

可以使用 GestureDetector 来监听更多的手势，例如：

单击事件

* onTapDown - 在特定区域发生点触屏幕的一个即时操作。
* onTapUp - 在特定区域发生触摸抬起的一个即时操作。
* onTap - 从点触屏幕之后到触摸抬起之间的单击操作。
* onTapCancel - 触发了 onTapDown，但未触发 tap 。

双击事件

* onDoubleTap - 用户在同一位置发生快速点击屏幕两次的操作。

长按事件

* onLongPress - 用户在同一位置长时间触摸屏幕的操作。

垂直拖动事件

* onVerticalDragStart - 用户手指接触屏幕，并且将要进行垂直移动事件。
* onVerticalDragUpdate - 用户手指接触屏幕，已经开始垂直移动，且会持续进行移动。
* onVerticalDragEnd - 用户之前手指接触了屏幕并发生了垂直移动操作，并且停止接触前还在以一定的速率移动。

水平拖动事件

* onHorizontalDragStart - 用户手指接触屏幕，并且将要进行水平移动事件。
* onHorizontalDragUpdate - 用户手指接触屏幕，已经开始水平移动，且会持续进行移动。
* onHorizontalDragEnd - 用户手指接触了屏幕并发生了水平移动操作，并且停止接触前还在以一定的速率移动。

## 8. 主题和文字

> 8.1 如何设置应用主题？

为了充分发挥应用中 Material Components 的优势，声明一个顶级的 widget，MaterialApp，来作为你的应用 入口。MaterialApp 是一个封装了大量常用 Material Design 组件的 widget。它基于 WidgetsApp 添加了 Material 的相关功能。

定义所有子组件颜色和样式，可以直接传递 ThemeData 对象给 MaterialApp widget。例如，在下面的代码中，primary swatch 被设置为蓝色，而文本选中后的颜色被设置为红色。

```
class SampleApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sample App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        textSelectionColor: Colors.red
      ),
      home: SampleAppPage(),
    );
  }
}
```

> 8.2 如何给 Text widget 设置自定义字体？

把字体放到一个文件夹中，然后在 pubspec.yaml 文件中引用它，就和引用图片一样。

> 8.3 我怎么给我的 Text widget 设置样式？

除了字体以外，你也可以自定义 Text widget 的其他样式。Text widget 接收一个 TextStyle 对象的参数，可以指定很多参数。

## 9. 表单输入

> 9.1 Flutter 中如何使用表单？我怎么拿到用户的输入？

Flutter 的其他部分一样，表单处理要通过特定的 widget 来实现。如果你有一个 TextField 或者 TextFormField， 你可以通过 TextEditingController 来 获取用户的输入。

> 9.2 Text field 中的 placeholder 相当于什么？

在 Flutter 里，通过向 Text widget 传递一个 InputDecoration 对象，你可以轻易的显示文本框的提示信息，或是 placeholder。

> 9.3 如何展示验证错误信息？

就和显示提示信息一样，你可以通过向 Text widget 传递一个 InputDecoration 来实现。

## 10. 和硬件、第三方服务以及系统平台交互

> 10.1 如何与系统平台以及平台原生代码进行交互？

Flutter 提供了用来和宿主 ViewController 通信 和交换数据的 platform channels。 platform channels 本质上是一个桥接了 Dart 代码与宿主 ViewController 和 iOS 框架的异步通信模型。你可以通过 platform channels 来执行原生代码的方法，或者获取设备的传感器信息等数据。

除了直接使用 platform channels 之外，也可以使用一系列包含了原生代码和 Dart 代码，实现了特定功能的现有插件。例如，你在 Flutter 中可以直接使用插件来访问相册或是设备摄像头，而不需要自己重新集成。Pub 是一个 Dart 和 Flutter 的开源包仓库，你可以在这里找到需要的插件。有些包可能支持集成 iOS 或 Android，或两者皆有。

> 10.2 如何访问 GPS 传感器？

使用 geolocator 插件，这一插件由社区提供。

> 10.3 如何访问摄像头？

image_picker 是常用的访问相机的插件。

> 10.4 我怎么登录 Facebook？

登录 Facebook 可以使用 flutter_facebook_login 社区插件。

> 10.5 如何集成 Firebase 功能？

大多数 Firebase 特性被  first party plugins 包含了。

> 10.6 如何构建自己的插件？

如果有一些 Flutter 和遗漏的平台特性，可以 根据 developing packages and plugins 构建自己的插件。

## 11. 数据库和本地存储

> 11.1 Flutter 中如何访问 UserDefaults？

在 Flutter 里，可以使用 Shared Preferences 插件来实现相同的功能。这个插件封装了 UserDefaults 以及 Android 里类似的 SharedPreferences。

> 11.2 CoreData 相当于 Flutter 中的什么？

在 iOS 里，你可以使用 CoreData 来存储结构化的数据。这是一个基于 SQL 数据库的上层封装，可以使关联模型的查询变得更加简单。

在 Flutter 里，可以使用 SQFlite 插件来实现这个功能。

## 12. 通知

> 12.1 如何设置推送通知？

在 Flutter 里，使用 firebase_messaging 插件来实现这个功能。
