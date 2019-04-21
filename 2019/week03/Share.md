# 事件传递

## 原理

只有继承了UIResponder的对象才能接受并处理事件。

* UIApplication
* UIViewController
* UIView

UIResponder中提供了以下4个对象方法来处理触摸事件

```
// UIResponder内部提供了以下方法来处理事件触摸事件
- (void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event;
- (void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event;
- (void)touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event;
- (void)touchesCancelled:(NSSet *)touches withEvent:(UIEvent *)event;
// 加速计事件
- (void)motionBegan:(UIEventSubtype)motion withEvent:(UIEvent *)event;
- (void)motionEnded:(UIEventSubtype)motion withEvent:(UIEvent *)event;
- (void)motionCancelled:(UIEventSubtype)motion withEvent:(UIEvent *)event;
// 远程控制事件
- (void)remoteControlReceivedWithEvent:(UIEvent *)event;
```

### 事件响应链

UIApplication -> window -> UIViewController -> 寻找处理事件最合适的view（遍历子视图）

### 获取最合适的View

返回合适的View

```
- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event{ 
   return self;//根据业务需求，返回合适的View，可以是自己，也可以是子视图
}
```

return nil的含义：最适合的View是父视图

pointInside：点是否在窗口上

hitTest:withEvent:实现逻辑

```
- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event{
    // 1.判断下窗口能否接收事件
     if (self.userInteractionEnabled == NO || self.hidden == YES ||  self.alpha <= 0.01) return nil; 
    // 2.判断下点在不在窗口上 
    // 不在窗口上 
    if ([self pointInside:point withEvent:event] == NO) return nil; 
    // 3.从后往前遍历子控件数组 
    int count = (int)self.subviews.count; 
    for (int i = count - 1; i >= 0; i--)     { 
    	// 获取子控件
    	UIView *childView = self.subviews[i]; 
    	// 坐标系的转换,把窗口上的点转换为子控件上的点 
    	// 把自己控件上的点转换成子控件上的点 
    	CGPoint childP = [self convertPoint:point toView:childView]; 
    	UIView *fitView = [childView hitTest:childP withEvent:event]; 
    	if (fitView) {
    	// 如果能找到最合适的view 
    	return fitView; 
    	}
    } 
    // 4.没有找到更合适的view，也就是没有比自己更合适的view 
    return self;
}
```

## 应用

### UITouch对象

#### UITouch的属性

```
@property(nonatomic,readonly,retain) UIWindow *window; // 触摸产生时所处的窗口
@property(nonatomic,readonly,retain) UIView *view; // 触摸产生时所处的视图
@property(nonatomic,readonly) NSUInteger tapCount; // 短时间内点按屏幕的次数，可以根据tapCount判断单击、双击或更多的点击
@property(nonatomic,readonly) NSTimeInterval timestamp; // 记录了触摸事件产生或变化时的时间，单位是秒
@property(nonatomic,readonly) UITouchPhase phase; // 当前触摸事件所处的状态
```

#### UITouch的方法

```
- (CGPoint)locationInView:(UIView *)view;// 返回值表示触摸在view上的位置，这里返回的位置是针对view的坐标系的（以view的左上角为原点(0, 0)）
- (CGPoint)previousLocationInView:(UIView *)view;// 该方法记录了前一个触摸点的位置
```

#### 获取坐标

```
- (void)touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event{
	UITouch *touch = [touches anyObject];
	CGPoint location = [touch locationInView:self];
}
```

### 点击View时，子视图响应而父视图不响应

```
- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event{
    UIView *view = [super hitTest:point withEvent:event];
    if (view == self) {
        return nil;
    }
    return view;
}
```

