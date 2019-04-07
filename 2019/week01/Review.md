# [iOS11 Files文件应用程序开发](http://www.cocoachina.com/ios/20190326/26646.html)

最近英文文章看得很少，意识到离技术高手还有很长的路要走。这第一篇，还是中文文章，后续尽快阅读英文文章，迈向高手之路。

> 拷贝模式:将qq或微信的文档拷贝到自己项目中
>
> 存储模式:将qq或微信的文档存储到“文件”中

## 拷贝模式

1. 在info.plist文件中，添加`Document Types`
2. 在 `item0` 中添加 `key Document Content Type UTIs`。public.data 代表可以接受任何类型的文件。
3. 在`Appdelegate`中，处理传入的URL

```
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url options:(nonnull NSDictionary<NSString *,id> *)options
{
    // dosomething
    if ([url.absoluteString hasPrefix:@"file:///private"]) {
        NSString *path = [[NSHomeDirectory() stringByAppendingString:@"Documents/Files/"] stringByAppendingPathComponent:url.lastPathComponent];
        NSData *data = [NSData dataWithContentsOfFile:path];
    }
    return YES;
}
```

## 存储模式

1. 在`info.plist`文件中，添加`Supports Document Browser`
2. 其他App，如qq或者微信，可以存在在Files。选择我们开发的App目录。
3. 在我们App中，如果需要访问其他App存储的文件，使用`UIDocumentPickerViewController`可进行操作

```
NSArray *documentTypes = @[@"public.data"];//可指定具体类型，public.data代表所有数据类型
    UIDocumentPickerViewController *vc = [[UIDocumentPickerViewController alloc] initWithDocumentTypes:documentTypes inMode:UIDocumentPickerModeOpen];
    vc.delegate = self;
    [self presentViewController:vc animated:YES completion:nil];
```


