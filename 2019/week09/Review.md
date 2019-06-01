# [iOS内购](https://www.jianshu.com/p/9531a85ba165)

> IAP 是客户端程序员必须掌握的知识。而苹果的 IAP 支付，在开发的过程当中，由于沙盒账户、网络等问题，经常会遇到不少的坑，有必要把这部分知识再回顾回顾。

## IAP 支付类型

1. 消耗型商品：只可使用一次的产品，使用之后即失效，必须再次购买。
2. 非消耗型商品：只需购买一次，不会过期或随着使用而减少的产品。
3. 自动续期订阅：允许用户在固定时间段内购买动态内容的产品。除非用户选择取消，否则此类订阅会自动续期。
4. 非续期订阅：允许用户购买有时限性服务的产品。此 App 内购买项目的内容可以是静态的。此类订阅不会自动续期。

## 内购流程

1. 用户向苹果服务器发起购买请求，收到购买完成的回调（购买完成后会把钱从申请内购的银行卡内扣除）
2. 购买成功流程结束后, 向服务器发起验证凭证（app端自己也可以不依靠服务器自行验证）
3. 自己的服务器工作分4步
    1. 接收ios端发过来的购买凭证。
    2. 判断凭证是否已经存在或验证过，然后存储该凭证。
    3. 将该凭证发送到苹果的服务器（区分沙盒环境还是正式环境）验证，并将验证结果返回给客户端。
    4. 修改用户相应的会员权限或发放虚拟物品。

## 代码实现

* 自动订阅类型，app开始运行时，一定要添加监听

`[[SKPaymentQueue defaultQueue] addTransactionObserver:self];
`

* 订单结束后一定要执行 finishTransaction 操作

`[[SKPaymentQueue defaultQueue] finishTransaction:transaction];
`

* 开始调起支付流程，请求商品信息，这里需要用到SKProductsRequestDelegate，它是商品请求回调，可告诉你有没有这个商品

`SKProductsRequest * request = [[SKProductsRequest alloc] initWithProductIdentifiers:set];`

> * SKProductsRequest 是苹果封装好的一个对象，该对象有两个属性。
> * 属性 products 是一个数组，代表的是你获取到的所有商品信息，每个商品都是一个数组元素。
> * 属性 invalidProductIdentifiers 是无效的商品id的数组，此id对应的是你在苹果后台构建的商品id。

* 判断购买结果，这里需要用到 SKPaymentTransactionObserver，SKPaymentTransactionObserver 是交易观察者，用来告诉你交易进行到哪个步骤了。

`- (void)paymentQueue:(SKPaymentQueue *)queue updatedTransactions:(NSArray *)transaction `

## 遇到问题

1. Upgrades and Plan Changes升级和计划变更

用户可以在App Store或您应用的界面中的帐户设置中管理他们的订阅。

可以查看收据的“订阅自动续订首选项”字段，以了解用户选择的任何计划更改，这些更改将在下一个续订日期生效。

2. Expiration and Renewal到期和续订
3. Cancellation消除

## 服务端验证

自动续订订阅和其他类型的区别还有必须在 App Store Connect 中生成一个共享密钥。

## 沙盒账户

沙盒账号的续订，如果一直打开着app，可能过了5分钟续订周期也不会收到通知，最好是杀死app，5分钟后重新启动，这样就会收到续订的通知了。

## 审核问题

1. 自动续订订阅的说明一定要有。
2. 不允许强制用户必须登录才能购买。


