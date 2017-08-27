# Block 进阶

## block细节
- 如果【block内部】使用【外部声明的强引用】访问【对象A】, 那么【block内部】会自动产生一个【强引用】指向【对象A】
- 如果【block内部】使用【外部声明的弱引用】访问【对象A】, 那么【block内部】会自动产生一个【弱引用】指向【对象A】

```objc
- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
{

    __weak typeof(self) weakSelf = self;

        self.blockTest = ^{

            __strong typeof(weakSelf) strongSelf = weakSelf;


            dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{

                NSLog(@"after------%@",weakSelf.data);

            });

        };

    self.blockTest();

    [self dismissViewControllerAnimated:YES completion:nil];

}

- (void)dealloc
{
    NSLog(@"dealloc-----");
}

2016-08-30 15:46:16.233 03-block开发使用场景（传值）[1495:112453] dealloc-----
2016-08-30 15:46:17.912 03-block开发使用场景（传值）[1495:112453] after------(null)


```

```objc
- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
{

    __weak typeof(self) weakSelf = self;

        self.blockTest = ^{

            __strong typeof(weakSelf) strongSelf = weakSelf;


            dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{

                NSLog(@"after------%@",strongSelf.data);

            });

        };

    self.blockTest();

    [self dismissViewControllerAnimated:YES completion:nil];

}

- (void)dealloc
{
    NSLog(@"dealloc-----");
}

2016-08-30 15:48:30.123 03-block开发使用场景（传值）[1515:114066] after------测试
2016-08-30 15:48:30.123 03-block开发使用场景（传值）[1515:114066] dealloc-----

```

- 如果是局部变量,Block是值传递
- 如果是静态变量,全局变量,__block修饰的变量,block都是指针传递(就是值可以改变)
```objc
    __block int a = 3;

    // 如果是局部变量,Block是值传递

    // 如果是静态变量,全局变量,__block修饰的变量,block都是指针传递

    void(^block)() = ^{

        NSLog(@"%d",a);

    };

    a = 5;

    block();
```


```objc
/*
    传值:1.只要能拿到对方就能传值

    顺传:给需要传值的对象,直接定义属性就能传值
    逆传:用代理,block,就是利用block去代替代理
 */

 - (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
{
    SecondViewController *second = [[SecondViewController alloc] init];

    second.block = ^(NSString *data){
        NSLog(@"%@",data);
    };

    [self presentViewController:second animated:YES completion:nil];

}

@interface SecondViewController : UIViewController

@property (nonatomic,strong) void(^block)(NSString * data);

@end

 if(self.block)
    {
        self.block(@"hellow");
    }

```

####block是不是一个对象?
 - 是一个对象

####如何判断当前文件是MRC,还是ARC
- 1.dealloc 能否调用super,只有MRC才能调用super
- 2.能否使用retain,release.如果能用就是MRC

###ARC管理原则:只要一个对象没有被强指针修饰就会被销毁,默认局部变量对象都是强指针,存放到堆里面

###MRC了解开发常识:
- 1.MRC没有strong,weak,局部变量对象就是相当于基本数据类型
- 2.MRC给成员属性赋值,一定要使用set方法,不能直接访问下划线成员属性赋值

####总结:只要block没有引用外部局部变量,block放在全局区

###MRC:管理block
- 只要Block引用外部局部变量,block放在栈里面.
- block只能使用copy,不能使用retain,使用retain,block还是在栈里面

###ARC:管理block
- 只要block引用外部局部变量,block放在堆里面
- block使用strong.最好不要使用copy
