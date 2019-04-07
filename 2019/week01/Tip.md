# Masonry使用

## 使用

```
//这个方法只会添加新的约束
[blueView mas_makeConstraints:^(MASConstraintMaker *make)  {

}];

// 这个方法会将以前的所有约束删掉，添加新的约束
[blueView mas_remakeConstraints:^(MASConstraintMaker *make) {

}];

// 这个方法将会覆盖以前的某些特定的约束
[blueView mas_updateConstraints:^(MASConstraintMaker *make) {

}];
```

## 约束的类型

1. 尺寸：width、height、size
2. 边界：left、leading、right、trailing、top、bottom
3. 中心点：center、centerX、centerY
4. 边界：edges
5. 偏移量：offset、insets、sizeOffset、centerOffset
6. priority()约束优先级（0~1000），multipler乘因数, dividedBy除因数

## equalTo 和 mas_equalTo的区别

`mas_equalTo`只是对其参数进行了一个BOX(装箱) 操作，目前支持的类型：数值类型（NSNumber）、 点（CGPoint）、大小（CGSize）、边距（UIEdgeInsets），而`equalTo`：这个方法不会对参数进行包装。

## 一行中多个label

在日常的代码开发中，经常会遇到，一行中，有多个label，有的label，需要完全显示，而有的label，则不需要完全显示，则需要设置label的优先级。

```
[label1 setContentCompressionResistancePriority:UILayoutPriorityRequired forAxis:UILayoutConstraintAxisHorizontal];
[label2 setContentCompressionResistancePriority:UILayoutPriorityRequired forAxis:UILayoutConstraintAxisHorizontal];
[label3 setContentCompressionResistancePriority:UILayoutPriorityDefaultLow forAxis:UILayoutConstraintAxisHorizontal];
```


