# [Flutter | 状态管理——BLoC](https://juejin.im/post/5bb6f344f265da0aa664d68a)

## 为什么要状态管理

在Stateful widget中，描述信息被放到state中，而组件中，只持有一些不可变的信息。当状态发生改变时，通过设置状态setState，重新绘制builder。

当App变复杂时，setState调用的次数会显著增加。因此希望，在多个页面中，共享状态，来管理App的状态。

## bloc是什么

BLoC代表业务逻辑组件`Business Logic Component`

- 用StreamBuilder包裹有状态的部件，streambuilder将会监听一个流
- 这个流来自于BLoC
- 有状态小部件中的数据来自于监听的流。
- 用户交互手势被检测到，产生了事件。例如按了一下按钮。
- 调用bloc的功能来处理这个事件
- 在bloc中处理完毕后将会吧最新的数据add进流的sink中
- StreamBuilder监听到新的数据，产生一个新的snapshot，并重新调用build方法
- Widget被重新构建

## demo

不同页面，使用BLoC共享App信息。对同一个数字做累加操作，不同页面，都显示最后结果。

### 1. 创建bloc

```
import 'dart:async';

class CountBLoC {
 int _count;
 StreamController<int> _countController;

 CountBLoC() {
   _count = 0;
   _countController = StreamController<int>();
 }
 
 Stream<int> get value => _countController.stream;

 increment() {
   _countController.sink.add(++_count);
 }

 dispose() {
   _countController.close();
 }
}

```

### 2. 创建bloc实例

```
class BlocProvider extends InheritedWidget {
  CountBLoC bLoC = CountBLoC();

  BlocProvider({Key key, Widget child}) : super(key: key, child: child);

  @override
  bool updateShouldNotify(_) => true;

  static CountBLoC of(BuildContext context) =>
      (context.inheritFromWidgetOfExactType(BlocProvider) as BlocProvider).bLoC;
}
```

### 3. 在页面中使用streambuilder

```
StreamBuilder<int>(
            stream: bloc.value,
            initialData: 0,
            builder: (BuildContext context, AsyncSnapshot<int> snapshot) {
              return Text(
                'You hit me: ${snapshot.data} times',
                style: Theme.of(context).textTheme.display1,
              );
            })
```

