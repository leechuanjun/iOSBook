# Block注意事项
##本小节知识点:
1. 【掌握】Block注意事项

---


##1.Block注意事项
- 在block内部可以访问block外部的变量

```objc
int  a = 10;
void (^myBlock)() = ^{
    NSLog(@"a = %i", a);
    }
myBlock();
输出结果: 10
```

- block内部也可以定义和block外部的同名的变量(局部变量),此时局部变量会暂时屏蔽外部

```objc
int  a = 10;
void (^myBlock)() = ^{
    int a = 50;
    NSLog(@"a = %i", a);
    }
myBlock();
输出结果: 50
```

- 默认情况下, Block内部不能修改外面的局部变量

```objc
int b = 5;
void (^myBlock)() = ^{
    b = 20; // 报错
    NSLog(@"b = %i", b);
    };
myBlock();

```
- Block内部可以修改使用__block修饰的局部变量
```objc
 __block int b = 5;
void (^myBlock)() = ^{
    b = 20;
    NSLog(@"b = %i", b);
    };
myBlock();
输出结果: 20
```


