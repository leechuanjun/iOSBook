#TabBar
##自定义TabBarController
- 因为UI妹纸给的图片超出了系统支持的最大尺寸具体看官方文档: https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html
- 自定义TabBarController

- 1.移除系统tabBar子控件
    - 1.1 移除原系统的UItabBarButton
    - 1.2 子控键用UIButton代替
    - 1.3 切换子控制器 selectedIndex

- 2 切换控制器
    - 1.调用self.selectedIndex,方法切换子控制器
    - 2.用代理通知控制器我已经点击了,你要做什么那是你的事情(切换控制器)
    - 3.用button的tag传递索引

- 1、设置TabBar
```objc
-(void)setupTabBar
{
    //自定义TabBar
    ZJCTabBar *tabBar = [[ZJCTabBar alloc] init];
    //储存tabBarItem的可变数组赋值给自定义TabBar
    tabBar.tabBarItem = self.item;
    //利用KVC赋值给私有变量
    [self setValue:tabBar forKey:@"tabBar"];

    //利用RAC监听UITabBarButton(实质是监听添加在TabBar的Button)
    tabBar.buttonSubject = [RACSubject subject];

    @weakify(self)
    [tabBar.buttonSubject subscribeNext:^(NSNumber *num) {
        @strongify(self)

        self.selectedIndex = [num integerValue];
    }];
}
```

```objc
- (void)setupChildrenControllerWithimage:(NSString *)image selectImage:(NSString *)selectImage controller:(UIViewController *)vc navTitle:(NSString *)title
{
    //判断图片是否存在
    if(image.length && selectImage.length)
    {
        vc.tabBarItem.image = [UIImage imageNamed:image];
        vc.tabBarItem.selectedImage = [UIImage imageNamed:selectImage];
    }
    vc.navigationItem.title = title;

    //item为储存tabBarItem的可变数组
    [self.item addObject:vc.tabBarItem];

    // 特别处理的ViewController
    if ([vc isKindOfClass:[XMGHallTableViewController class]]) {
        ZJCHallNavigationController *nav = [[ZJCHallNavigationController alloc] initWithRootViewController:vc];
        [self addChildViewController:nav];
    }else{
    //一般的ViewController
        ZJCNavigationController *nav = [[ZJCNavigationController alloc] initWithRootViewController:vc];
        [self addChildViewController:nav];
    }
}
```

```objc
- (void)setupTabBarController
{

    XMGMyLotteryViewController *Lottery = [[XMGMyLotteryViewController alloc] init];
    [self setupChildrenControllerWithimage:@"TabBar_MyLottery_new" selectImage:@"TabBar_MyLottery_selected_new" controller:Lottery navTitle:@"我的彩票"];

    XMGArenaViewController *arena = [[XMGArenaViewController alloc] init];
    [self setupChildrenControllerWithimage:@"TabBar_Arena_new" selectImage:@"TabBar_Arena_selected_new" controller:arena navTitle:@"竞技场"];

    XMGHallTableViewController *hall = [[XMGHallTableViewController alloc] init];
    [self setupChildrenControllerWithimage:@"TabBar_Hall_new" selectImage:@"TabBar_Hall_selected_new" controller:hall navTitle:@"彩购大厅"];

    UIStoryboard *storyboard = [UIStoryboard storyboardWithName:NSStringFromClass([XMGDiscoverTableViewController class]) bundle:nil];
    XMGDiscoverTableViewController *discover = [storyboard instantiateInitialViewController];
    [self setupChildrenControllerWithimage:@"TabBar_Discovery_new" selectImage:@"TabBar_Discovery_selected_new" controller:discover navTitle:@"发现"];

    XMGHistoryTableViewController *history = [[XMGHistoryTableViewController alloc] init];
    [self setupChildrenControllerWithimage:@"TabBar_History_new" selectImage:@"TabBar_History_selected_new" controller:history navTitle:@"开奖信息"];
}
```

- 2、自定义TabBar

```objc
- (void)layoutSubviews
{
    //必须有[super layoutSubviews]，实现父类方法
    [super layoutSubviews];

    //移除旧的UItabBarButton
    NSInteger num = self.subviews.count;
    for (int i = 0 ; i < num ; i++) {
        UIView *view = self.subviews[0];
        [view removeFromSuperview];
    }

    //添加新的UItabBarButton，5为Button个数
    CGFloat buttonW = self.frame.size.width / 5;
    CGFloat buttonH = self.frame.size.height;

    //添加新的UITabBarButton，（实际是添加Button）
    for (int i = 0; i < self.tabBarItem.count; i++) {

        UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
        UITabBarItem *item = self.tabBarItem[i];

        [button setImage:item.image forState:UIControlStateNormal];
        [button setImage:item.selectedImage forState:UIControlStateSelected];
        button.tag = i;
        button.frame = CGRectMake(buttonW * i, 0, buttonW, buttonH);

        //设置默认的tabBarButton
        if(i == 0)
        {
            button.selected = YES;
            self.selectBtn = button;
        }

        //tabBarButton监听
        @weakify(self)
        [[button rac_signalForControlEvents:UIControlEventTouchUpInside] subscribeNext:^(UIButton *button) {

            @strongify(self)

            if (self.selectBtn != nil) {
                self.selectBtn.selected = NO;
            }
            button.selected = YES;
            self.selectBtn = button;

            //发送选着的tabIndesx
            if (self.buttonSubject) {
                NSNumber *num = [NSNumber numberWithInteger:button.tag];
                [self.buttonSubject sendNext:num];
            }
        }];

        [self addSubview:button];
    }
}

```
