# [iOS 布局](https://www.jianshu.com/p/7d0133f99fa7)

布局、显示和约束都遵循着相似的模式，例如他们更新的方式以及如何在 run loop 的不同时间点上强制更新。任一组件都有一个实际去更新的方法（layoutSubviews, draw, 和 updateConstraints），你可以重写来手动操作视图，但是任何情况下都不要显式调用。这个方法只在 run loop 的末端会被调用，如果视图被标记了告诉系统该视图需要被更新的标记的话。有一些操作会自动设置这个标志，但是也有一些方法允许您显式地设置它。对于布局和约束相关的更新，如果你等不到在 run loop 末端才更新（例如：其他行为依赖于新布局），有方法可以让你立即更新，并保证 “update layout” 标记被正确标记。下面的表格列出了任意组件会怎样更新及其对应方法。

你可以在 run loop 中的任意一点显式地调用 layoutIfNeeded 或者 updateConstraintsIfNeeded，需要记住，这开销会很大。在循环的末端是 update cycle，如果视图被设置了特定的 “update constraints”，“update layout” 或者 “needs display” 标记，在这节点会更新约束、布局以及展示。一旦这些更新结束，runloop 会重新启动。


Method Purposes | Layout | Display | Constraints
--------------- | ------ | ------- | ------------
override，不主动调用 | layoutSubViews | draw | updateConstraints
update cycle周期 | setNeedsLayout | setNeedsDisplay | setNeedsUpdateConstraints、invalidateIntrinsicContentSize
立即调用 | layoutIfNeed |  | updateConstranitsIfNeeded
何时触发 | addSubview、bounds改变、滑动 | 改变bounds | 激活/禁用/更新/移除约束

## RunLoop

iOS应用中，所有的用户交互都会被加入到一个事件队列中。用户输入 -> 代码处理 -> 主 RunLoop -> Update Cycle

## 布局

布局，一个视图的布局指的是它在屏幕上的的大小和位置。

* layoutSubViews。这个方法很开销很大，因为它会在每个子视图上起作用并且调用它们相应的 layoutSubviews 方法。
* setNeedsLayout。触发 layoutSubviews 调用的最省资源的方法就是在你的视图上调用 setNeedsLaylout 方法。调用这个方法代表向系统表示视图的布局需要重新计算。setNeedsLayout 方法会立刻执行并返回，但在返回前不会真正更新视图。
* layoutIfNeed。layoutIfNeeded 会立即调用 layoutSubviews 方法。如果你在同一个 run loop 内调用两次 layoutIfNeeded，并且两次之间没有更新视图，第二个调用同样不会触发 layoutSubviews 方法。

## 显示
    
* draw。UIView 的 draw 方法（本文使用 Swift，对应 Objective-C 的 drawRect）对视图内容显示的操作，类似于视图布局的 layoutSubviews ，但是不同于 layoutSubviews，draw 方法不会触发后续对视图的子视图方法的调用。
* setNeedsDisplay。这个方法类似于布局中的 setNeedsLayout 。它会给有内容更新的视图设置一个内部的标记，但在视图重绘之前就会返回。然后在下一个 update cycle 中，系统会遍历所有已标标记的视图，并调用它们的 draw 方法。

## 约束

* updateConstraints。自动布局中动态改变视图约束。
* setNeedUpdateConstraints。调用 setNeedsUpdateConstraints() 会保证在下一次更新周期中更新约束。
* updateConstranitsIfNeeded。如果它认为这些约束需要被更新，它会立即触发 updateConstraints() ，而不会等到 run loop 的末尾。
* invalidateIntrinsicContentSize。调用 invalidateIntrinsicContentSize() 会设置一个标记表示这个视图的 intrinsicContentSize 已经过期，需要在下一个布局阶段重新计算。






