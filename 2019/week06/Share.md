# ViewController结构

一谈到 iOS 设计模式，大家都知道的是 MVC 设计模式。MVC 是 iOS 开发的基础。MVC 简单来说，就是 Model、View、Controller。而苹果内置的开发框架中，就把 MVC 集成进来了，就是我要说的 ViewController。

ViewController，是 MVC 中的 Controller 层。负责管理 Model 及 View，将 Model层的数据，展示在 View 上。

新建 Xcode 工程后，我们新建一个 window 为系统 window。

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // 系统创建Windows
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    // 设置颜色
    self.window.backgroundColor = [UIColor whiteColor];
    // 设置为主窗口并显示
    [self.window makeKeyAndVisible];
    
    return YES;
}
```

## UIViewController

生命周期。init 实例化 ViewController 是，会先调用 initWithNibName 方法。而 storyboard 创建的 ViewController，只会调用 awakeFromNib 方法。

1. init/awakeFromNib/initWithNibName
2. viewDidLoad
3. viewWillAppear
4. viewDidAppear
5. viewWillDisappear
6. viewDidDisappear
7. dealloc

渲染周期

## UITabBarController

UITabBarController 管理一个 UITabBar（View）和 viewControllers 的集合。而 UITabBar 管理 UITabBarItem，显示底部的按钮。UITabBarItem 可以看成是数据 model。UITabBarItem 处理可以直接创建，赋值给 UITabBar，也可以直接给 ViewController.tabBarItem 的属性赋值，创建 UITabBarController 时，会自动创建 UITabBarItem。

```
UITabBarController *tabBarController = [[UITabBarController alloc] init];
    
ViewController *viewController1 = [[ViewController alloc] init];
viewController1.tabBarItem.title = @"title1";
ViewController *viewController2 = [[ViewController alloc] init];
viewController2.tabBarItem.title = @"title2";
ViewController *viewController3 = [[ViewController alloc] init];
viewController3.tabBarItem.title = @"title3";
ViewController *viewController4 = [[ViewController alloc] init];
viewController4.tabBarItem.title = @"title4";
    
tabBarController.viewControllers = @[viewController1, viewController2, viewController3];
```

UITabBar 的相关属性

* delegate：代理，tab切换事件
* items：底部按钮集合
* selectedItem：选中项
* tintColor
* barTintColor
* unselectedItemTintColor
* selectedImageTintColor
* itemWidth
* itemSpacing
* translucent

UITabBarItem 的相关属性

* title：标题
* image：图片
* imageInsets：图片边距
* setTitleTextAttributes：设置标题文本属性

UITabBar

## UINavigationController

UINavigationController 管理着一个 viewControllers 的栈。创建时，需要传入一个 rootViewController，作为最先入栈的 ViewController。通过 push 和 pop 进入入栈和出栈的操作。

```
UIViewController *viewController = [[UIViewController alloc] init];
viewController.title = @"navi";
UINavigationController *navigationController = [[UINavigationController alloc] initWithRootViewController:viewController];
self.window.rootViewController = navigationController;
navigationController.hidesBottomBarWhenPushed = YES;
```

UINavigationController 的常用方法及属性

* initWithRootViewController：初始化
* pushViewController：入栈
* popViewControllerAnimated：出栈
* topViewController：栈顶元素
* visibleViewController：如果有 modal view Controller，返回 modal，没有则返回 topViewController
* viewControllers：view controller 栈
* navigationBarHidden：导航栏是否隐藏
* setNavigationBarHidden：隐藏导航栏
* navigationBar：导航栏
* toolbarHidden：工具栏是否隐藏
* setToolbarHidden：隐藏工具栏
* toolbar：工具栏
* delegate：代理
* interactivePopGestureRecognizer：侧滑手势
* hidesBottomBarWhenPushed：push 时隐藏底部 bar

UINavigationBar 的相关属性

* delegate：代理
* translucent
* topItem
* backItem
* items
* tintColor
* barTintColor
* backgroundImage
* titleTextAttributes

UINavigationItem 的相关属性

* title
* titleView
* backBarButtonItem
* hidesBackButton
* leftBarButtonItems
* rightBarButtonItems
* leftBarButtonItem
* rightBarButtonItem

UIToolbar 的相关属性及方法

* items
* translucent
* tintColor
* barTintColor
* backgroundImage

UIBarButtonItem 的相关属性

* initWithTitle:style:target
* initWithBarButtonSystemItem:target:action:
* initWithCustomView:
* style
* width
* customView
* action
* target
* setBackgroundImage:forState:barMetrics

UINavigationController -> UINavigationBar -> UINavigationItem -> UIBarButtonItem

UINavigationController -> UIToolbar -> UIBarButtonItem


