# NSMutableString常用方法
##本小节知识点:
1. 【理解】NSMutableString常用方法
2. 【理解】字符串使用注意事项

---

##1.NSMutableString常用方法
- \- (void)appendString:(NSString *)aString;
    + 拼接aString到最后面

```objc
 NSMutableString *strM = [NSMutableString string];
    NSLog(@"strM = %@", strM);
    [strM appendString:@"lnj"];
    NSLog(@"strM = %@", strM);

```
- \- (void)appendFormat:(NSString *)format, ...;
    + 拼接一段格式化字符串到最后面

```objc
NSMutableString *strM = [NSMutableString string];
[strM appendFormat:@"/age is %i", 10];
```
- \- (void)deleteCharactersInRange:(NSRange)range;
    + 删除range范围内的字符串

```objc
    NSMutableString *strM = [NSMutableString stringWithString:@"http://www.520it.com"];
     // 一般情况下利用rangeOfString和deleteCharactersInRange配合删除指定内容
     NSRange range = [strM rangeOfString:@"http://"];
     [strM deleteCharactersInRange:range];
     NSLog(@"strM = %@", strM);
```
- \- (void)insertString:(NSString *)aString atIndex:(NSUInteger)loc;
    + 在loc这个位置中插入aString

```objc
    NSMutableString *strM = [NSMutableString stringWithString:@"www.520it.com"];
    [strM insertString:@"http://" atIndex:0];
    NSLog(@"strM = %@", strM);

```
- \- (void)replaceCharactersInRange:(NSRange)range withString:(NSString *)aString;
    + 使用aString替换range范围内的字符串

```objc
    NSMutableString *strM = [NSMutableString stringWithString:@"http://www.520it.com/lnj.png"];
    NSRange range = [strM rangeOfString:@"lnj"];
    [strM replaceOccurrencesOfString:@"lnj" withString:@"jjj" options:0 range:range];
    NSLog(@"strM = %@", strM);
```

---

##2.字符串使用注意事项
- @”lnj”这种方式创建的字符串始终是NSString,不是NSMutalbeString.所以下面的代码创建的还是NSString,此时使用可变字符串的函数,无法操作字符串。

```objc
NSMutalbeString *s1 = @”lnj”;
// 会报错
[strM insertString:@"my name is " atIndex:0];
```
---
