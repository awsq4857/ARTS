# App架构

## App是什么

在讨论App架构前，我们先想想，App是什么，做了什么。

App简单来讲，就是请求服务器数据，在手机客户端进行展示，并能响应用户的操作。

从这个意义上来讲，App分为几部分：

1. 数据请求
2. 数据展示
3. 数据交互

## App代码结构

根据上面的理解，我们可以把代码分成以下几层：

1. 网络层。进行数据请求。这一层涉及，model定义、归档；网络请求、请求header、post、get、Response解析、object实例化。
2. 数据处理层。DataCenter/Service，对数据进行加工。如数据格式的校验、排序。
3. 控制层。Controller，进行业务逻辑处理，响应交互及页面跳转。
4. View层。数据展示。可以是简单的View；也可以是封装的复杂View。
5. 路由层。当App业务逻辑复杂时，可以抽象出一个路由，专门负责App跳转。

App功能结构

模块化。当App越来越复杂时，可以按模块划分App。
组件化。将可复用的功能，封装成组件。模块是以业务划分，而组件是以功能划分为主。

## App优化 

App的优化，也是根据上面的部分进行优化

1. 网络层。DNS优化；json转Model等。
2. View层。数据缓存；图片缓存；视频缓存；减少cell的离屏渲染等。


