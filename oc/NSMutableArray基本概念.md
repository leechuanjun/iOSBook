# # NSMutableString基本概念
##本小节知识点:
1. 【理解】NSMutableArray介绍
2. 【理解】NSMutableArray基本用法
3. 【理解】NSMutableArray 错误用法

---

##1.NSMutableArray介绍
- 什么是NSMutableArray
    + NSMutableArray是NSArray的子类
    + NSArray是不可变的,一旦初始化完毕后,它里面的内容就永远是固定的, 不能删除里面的元素, 也不能再往里面添加元素
    + NSMutableArray是可变的,随时可以往里面添加\更改\删除元素

---

##2.NSMutableArray基本用法
- 创建空数组

```objc
NSMutableArray *arr = [NSMutableArray array];
```
- 创建数组,并且指定长度为5,此时也是空数组

```objc
NSMutableArray *arr2 = [[NSMutableArray alloc] initWithCapacity:5];
```
- 创建一个数组,包含两个元素

```objc
NSMutableArray *arr3 = [NSMutableArray arrayWithObjects:@"1",@"2", nil];
```
- 调用对象方法创建数组

```objc
NSMutableArray *arr4 = [[NSMutableArray alloc] initWithObjects:@"1",@"2", nil];
```

- \- (void)addObject:(id)object;
    + 添加一个元素

- \- (void)addObjectsFromArray:(NSArray *)array;
    + 添加otherArray的全部元素到当前数组中

- \- (void)insertObject:(id)anObject atIndex:(NSUInteger)index;
    + 在index位置插入一个元素

- \- (void)removeLastObject;
    + 删除最后一个元素

- \- (void)removeAllObjects;
    + 删除所有的元素

- \- (void)removeObjectAtIndex:(NSUInteger)index;
    + 删除index位置的元素

- \- (void)removeObject:(id)object;
    + 删除特定的元素

- \- (void)removeObjectsInRange:(NSRange)range;
    + 删除range范围内的所有元素

- \- (void)replaceObjectAtIndex:(NSUInteger)index withObject:(id)anObject;
    + 用anObject替换index位置对应的元素

- \- (void)exchangeObjectAtIndex:(NSUInteger)idx1 withObjectAtIndex:(NSUInteger)idx2;
    + 交换idx1和idx2位置的元素


---

##3.NSMutableArray 错误用法
- 不可以使用@[]创建可变数组

```objc
NSMutableArray *array = @[@"lnj", @"lmj", @"jjj"];
// 报错, 本质还是不可变数组
[array addObject:@“Peter”];
```
---
