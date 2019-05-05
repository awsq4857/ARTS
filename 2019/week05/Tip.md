# ViewController瘦身

随着版本的迭代，App的功能越来越多，ViewController的逻辑会越来越复杂，变得越来越难以维护。因此需要简化，将ViewController多余的逻辑抽离出来。

## 1. 抽离出DataSource

DataSource中的number、cell等事件，可以放在基类处理，上层只负责cell的赋值。cell的赋值，可以在cell的子类中赋值；也可以给不同的cell建立category，进行赋值。

## 2. 弱业务逻辑移至Model中

可以将弱业务逻辑移至model中，比如日期转换、图像裁剪、设定过滤器、数组的排序、数据的二次加工等。

## 3. 用DataCenter，进行数据处理

建立中间的DataCenter层或者store服务层，进行网络请求及数据处理、存储。

## 4. View封装，简化ViewController中View的创建

将复杂的View进行封装，简化ViewController中，View的逻辑处理。

## 5. 使用中介者模式，或者路由框架，统一处理跳转

借助中介者模式或者路由框架，实现统一的页面跳转。而不是在ViewController中，直接跳转页面。
