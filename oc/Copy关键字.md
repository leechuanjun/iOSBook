# @property中的copy关键字
##本小节知识点:
1. 【理解】@property中的copy的作用
2. 【理解】@property内存管理策略选择

---

##1.@property中的copy的作用
- 防止外界修改内部的值

```objc
    @interface Person : NSObject
    @property (nonatomic, retain) NSString *name;
    @end
```

```objc
    NSMutableString *str = [NSMutableString stringWithFormat:@"lnj"];

    Person *p = [[Person alloc] init];
    p.name = str;
    // person中的属性会被修改
    [str appendString:@" cool"];
    NSLog(@"name = %@", p.name);
```

- 防止访问对象对象已经释放
    + 不用copy情况
```objc
    Person *p = [[Person alloc] init];
    p.name = @"lnj";
    Dog *d = [[Dog alloc] init];
    d.age = 10;
    NSLog(@"retainCount = %lu", [d retainCount]); // 1
    p.pBlock = ^{
        // 报错, 调用之前就销毁了
        NSLog(@"age = %d", d.age);
    };
    [d release]; // 0
    p.pBlock();
    [p release];
```
    + 用copy情况

```objc
    Person *p = [[Person alloc] init];
    p.name = @"lnj";
    Dog *d = [[Dog alloc] init];
    d.age = 10;
    NSLog(@"retainCount = %lu", [d retainCount]); // 1
    p.pBlock = ^{
        // 会对使用到的外界对象进行一次retain
        NSLog(@"age = %d", d.age);
        NSLog(@"retainCount = %lu", [d retainCount]); // 1
    };
    [d release]; // 1
    p.pBlock();
    [p release];
```
---

##2.@property内存管理策略选择
- 非ARC
    + 1> copy : 只用于NSString\block
    + 2> retain : 除NSString\block以外的OC对象
    + 3> assign :基本数据类型、枚举、结构体(非OC对象),当2个对象相互引用,一端用retain,一端用assign

- ARC
    + 1> copy : 只用于NSString\block
    + 2> strong : 除NSString\block以外的OC对象
    + 3> weak : 当2个对象相互引用,一端用strong,一端用weak
    + 4> assgin : 基本数据类型、枚举、结构体(非OC对象)
---
