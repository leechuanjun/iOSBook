#UICollection 实现 九宫格

- 补充剩下的格子

```objc
  // 判断下缺几个(col 总列数 ，count 总个数，exter剩下的个数)
    exter =  col - ( count % col )

    #pragma mark - 处理请求完成数据
- (void)resloveData
{
    NSInteger count = self.squareItems.count;
    NSInteger exter = count % cols;
    if (exter) {
        exter = cols - exter;
        for (int i = 0; i < exter; i++) {
            XMGSquareItem *item = [[XMGSquareItem alloc] init];
            [self.squareItems addObject:item];
        }
    }

}
```

##计算有多少行
```objc
   // 设置collectionView 计算collectionView高度 = rows * itemWH
        // Rows = (count - 1) / cols + 1  3 cols4
        NSInteger count = self.itemArray.count;
        NSInteger row = (count - 1) / col + 1;
```

##UICollectionView 初始化
- 特别注意UICollectionView 只能用register的方法来注册cell

```objc
- (void)setupCollectionView
{
    UICollectionViewFlowLayout *flowLayout = [[UICollectionViewFlowLayout alloc] init];

    CGFloat col = 4;
    CGFloat width = ([UIScreen mainScreen].bounds.size.width - col - 1) / 4;

    flowLayout.minimumLineSpacing = 1;
    flowLayout.minimumInteritemSpacing = 1;
    flowLayout.itemSize = CGSizeMake(width, width);

    UICollectionView *collectionView = [[UICollectionView alloc] initWithFrame:CGRectMake(0, 0, [UIScreen mainScreen].bounds.size.width, 300) collectionViewLayout:flowLayout];

    collectionView.backgroundColor = [UIColor redColor];
    collectionView.dataSource = self;
    collectionView.delegate = self;

    collectionView.showsVerticalScrollIndicator = NO;
    collectionView.scrollEnabled = NO;

    self.collectionView = collectionView;

    [collectionView registerNib:[UINib nibWithNibName:NSStringFromClass([ZJCCollectionViewCell class]) bundle:nil] forCellWithReuseIdentifier:CollectID];

    self.tableView.tableFooterView = collectionView;
}

```
