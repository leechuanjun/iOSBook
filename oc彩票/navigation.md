#navigation
##设置导航条内容
- 1.包装成导航控制器 让普通的控制器成为导航控制器的根控制器
- 2.通过[navigationBar setBackgroundImage:] 设置背景图片,
- 3.UIBarMetrics模式
    - 1.UIBarMetricsDefault,导航控制器控制器的子控制器的view的大小 = 屏幕的高度 64 如果要想设置导航条的背景图片,只能用这种模式
    - 2.UIBarMetricsCompact,导航控制器的子控制器的view的大小 = 屏幕的高度,这种模式下如果设置了导航条的背景图片,没有效果
- 4.设置导航条的颜色标题
    - 1.TextAttributes 复文本
    - 2.通过[navigationBar setTitleTextAttributes:] 设置文字属性
NSForegroundColorAttributeName 设置字体颜色
NSFontAttributeName 设置字体大小

##自定义导航控制器
- 1.如果在viewDidLoad 里面调用会调用5次(因为创建了5个导航控制器),如果想调用一次就在initialize里面设置
- 2.initialize方法
当前类或者他的子类第一次使用的时候调用
作用:初始化一个类
- 3.load方法
当程序一启动的时候就会调用,当前类只会调用一次
作用: 把类加载进内存,放在代码区
- 4.一般不在load方法里面写东西,因为load方法是在程序一启动的时候调用的,这个时候自动释放池还没有创建,所以创建出来的对象需要手动释放
- 5.appearance 导航条的标志
    - 1.appearance是一个协议,只要遵守这个协议的类,都具备这个功能
    - 2.UIButton UILabel 也有这个功能
    - 3.如果用appearance就会导致重大bug,整个工程的导航条都一样
    - 4.我们只要管好自己的事情就可以了 谁用我的导航控制器,我就改下面的类的导航条
    - 5.采取这种方法
```objc
// appearanceWhenContainedIn获取当前类的标志
  UINavigationBar *bar = [UINavigationBar appearanceWhenContainedIn:self, nil];
  // iOS9以后新出的方法
  // Classes 要获取哪些类的属性
  UINavigationBar *bar = [UINavigationBar appearanceWhenContainedInInstancesOfClasses:@[self]];
```

```objc
    // 导航条标志
UINavigationBar *bar = [UINavigationBar appearance];
```


```objc

+ (void)initialize
{
    [self setupNav];
}

//设置返回手势
- (void)setPanGesture
{
    //获取系统手势target
    id target = self.interactivePopGestureRecognizer.delegate;

    // 禁止系统的手势
    self.interactivePopGestureRecognizer.enabled = NO;

    //把系统处理返回手势的方法设为Action
    UIPanGestureRecognizer *panGesture = [[UIPanGestureRecognizer alloc] initWithTarget:target action:@selector(handleNavigationTransition:)];

    [self.view addGestureRecognizer:panGesture];

    //设置手势代理
    panGesture.delegate = self;
}

//初始化nav,统一设置Navciton
+ (void) setupNav
{
    UINavigationBar *bar = [UINavigationBar appearanceWhenContainedInInstancesOfClasses:@[self]];

    [bar setBackgroundImage:[UIImage imageNamed:@"NavBar64"] forBarMetrics:UIBarMetricsDefault];

    NSMutableDictionary *dict = [NSMutableDictionary dictionary];

    //字体大小
    dict[NSFontAttributeName] = [UIFont boldSystemFontOfSize:22];
    // 字体颜色
    dict[NSForegroundColorAttributeName] = [UIColor whiteColor];

    [bar setTitleTextAttributes:dict];
}

#pragma mark - "pushViewController"
- (void)pushViewController:(UIViewController *)viewController animated:(BOOL)animated
{
    //必须写在  [super pushViewController:viewController animated:YES];

    if (self.childViewControllers.count > 0) { // 非跟控制器就隐藏底部TabBar
        viewController.hidesBottomBarWhenPushed = YES;
    }

    [super pushViewController:viewController animated:YES];

    //self.viewControllers 表示压栈的控制器
    //设置返回键
    if(self.viewControllers.count > 1)
    {
        UIButton *backBtn = [UIButton buttonWithType:UIButtonTypeCustom];
        [backBtn setImage:[UIImage imageNamed:@"NavBack"] forState:UIControlStateNormal];
        [backBtn setTitle:@"返回" forState:UIControlStateNormal];
        //调整返回键的位置
        backBtn.imageEdgeInsets = UIEdgeInsetsMake(0, -10, 0, 0);
        [backBtn sizeToFit];

        [[backBtn rac_signalForControlEvents:UIControlEventTouchUpInside] subscribeNext:^(id x) {
            [viewController.navigationController popViewControllerAnimated:YES];
        }];

        UIBarButtonItem *leftBtn = [[UIBarButtonItem alloc] initWithCustomView:backBtn];

        viewController.navigationItem.leftBarButtonItem = leftBtn;
    }
}

//代理设置
- (BOOL)gestureRecognizerShouldBegin:(UIGestureRecognizer *)gestureRecognizer
{
    //判断是否入栈控制器大于1，是就开启手势
    return self.childViewControllers.count > 1;
}
```
