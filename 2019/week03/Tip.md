# iOS字符集处理

日常开发中，经常会涉及到特殊字符处理。比如，用户注册时，昵称长度的限制，如果最后一位正好是emoji表情时，根据长度进行裁剪时，会出现乱码的情况。

## Emoji表情处理

### 1. Emoji表情的裁剪

```
// 给定最大长度，计算字符串长度
NSRange rangeRange = [text rangeOfComposedCharacterSequencesForRange:NSMakeRange(0, maxLength)];
// 裁剪字符串
newText = [text substringWithRange:rangeRange];
```

### 2. 是否包含Emoji表情

```
//是否含有表情
- (BOOL)stringContainsEmoji:(NSString *)string {
    __block BOOL returnValue = NO;
    [string enumerateSubstringsInRange:NSMakeRange(0, [string length])
                               options:NSStringEnumerationByComposedCharacterSequences
                            usingBlock:^(NSString *substring, NSRange substringRange, NSRange enclosingRange, BOOL *stop) {
                                const unichar hs = [substring characterAtIndex:0];
                                if (0xd800 <= hs && hs <= 0xdbff) {
                                    if (substring.length > 1) {
                                        const unichar ls = [substring characterAtIndex:1];
                                        const int uc = ((hs - 0xd800) * 0x400) + (ls - 0xdc00) + 0x10000;
                                        if (0x1d000 <= uc && uc <= 0x1f77f) {
                                            returnValue = YES;
                                        }
                                    }
                                } else if (substring.length > 1) {
                                    const unichar ls = [substring characterAtIndex:1];
                                    if (ls == 0x20e3) {
                                        returnValue = YES;
                                    }
                                } else {
                                    if (0x2100 <= hs && hs <= 0x27ff) {
                                        if (0x278b <= hs && hs <=0x2792) {//九宫格输入法处理
                                            returnValue = NO;
                                        }else if (0x263b == hs){//笑脸，看业务需求，是否过滤
                                            returnValue = NO;
                                        }else{
                                            returnValue = YES;
                                        }
                                    } else if (0x2B05 <= hs && hs <= 0x2b07) {
                                        returnValue = YES;
                                    } else if (0x2934 <= hs && hs <= 0x2935) {
                                        returnValue = YES;
                                    } else if (0x3297 <= hs && hs <= 0x3299) {
                                        returnValue = YES;
                                    } else if (hs == 0xa9 || hs == 0xae || hs == 0x303d || hs == 0x3030 || hs == 0x2b55 || hs == 0x2b1c || hs == 0x2b1b || hs == 0x2b50) {
                                        returnValue = YES;
                                    }
                                }
                            }];
    return returnValue;
}
```

## 字符串处理

### 字符集

iOS可以使用`NSCharacterSet`和`NSMutableCharacterSet`来处理字符集。如果不想使用字符集类处理字符串，可以自己写正则表达式。

```
1.controlCharacterSet //控制符的字符集
2.whitespaceCharacterSet //空格的字符集
3.whitespaceAndNewlineCharacterSet //空格和换行符的字符集
4.decimalDigitCharacterSet //十进制数字的字符集
5.letterCharacterSet //字母的字符集
6.lowercaseLetterCharacterSet //小写字母的字符集
7.uppercaseLetterCharacterSet //大写字母的字符集
8.nonBaseCharacterSet //非基础的字符集
9.alphanumericCharacterSet //字母和数字的字符集
10.decomposableCharacterSet //可分解
11.illegalCharacterSet //非法的字符集
12.punctuationCharacterSet //标点的字符集
13.capitalizedLetterCharacterSet //首字母大写的字符集
14.symbolCharacterSet //符号的字符集
15.newlineCharacterSet //换行符的字符集
```

### 字符的查找、替换

可以通过字符集，进行字符串的查找、分隔、替换

```
//返回一个指定字符集分隔开的子字符串数组
- (NSArray<NSString *> *)componentsSeparatedByCharactersInSet:(NSCharacterSet *)separator NS_AVAILABLE(10_5, 2_0);
//返回一个去除两端指定字符集的字符串
- (NSString *)stringByTrimmingCharactersInSet:(NSCharacterSet *)set;
//返回指定字符集在当前字符串中的第一个符合条件的范围
- (NSRange)rangeOfCharacterFromSet:(NSCharacterSet *)searchSet;
```

通过`rangeOfCharacterFromSet`查找字符串，然后进行相应的逻辑处理

```
if ([string rangeOfCharacterFromSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]].location != NSNotFound) {
    // 字符串中包含换行符
}
```

## 参考链接

1. [NSCharacterSet-字符集使用总结](https://blog.csdn.net/wangyanchang21/article/details/53415650?utm_source=blogxgwz6)
2. [NSCharacterSet 对于字符串的处理](https://blog.csdn.net/wiki_su/article/details/52516016)
3. [iOS 自带九宫格拼音键盘与 Emoji 表情之间的坑](https://kangzubin.com/nine-keyboard-emoji/)