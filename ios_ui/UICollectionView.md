#UICollectionView 创建
参考博客http://www.cnblogs.com/wayne23/p/4013522.html
```objc
UICollectionViewFlowLayout *flowLayout= [[UICollectionViewFlowLayout alloc]init];
self.myCollectionView = [[UICollectionView alloc] initWithFrame:CGRectMake(20, 20, 250, 350) collectionViewLayout:flowLayout];
self.myCollectionView.backgroundColor = [UIColor grayColor];
[self.myCollectionView registerClass:[UICollectionViewCell class] forCellWithReuseIdentifier:@“myCell"];
self.myCollectionView.delegate = self;
self.myCollectionView.dataSource = self;

[self.view addSubview:self.myCollectionView];
```

#UICollectionViewLayout 流式布局
- UICollectionViewLayout决定了UICollectionView如何显示在界面上，Apple提供了一个最简单的默认layout对象：UICollectionViewFlowLayout。
- itemSize属性(设定全局的Cell尺寸)

```objc
//设定全局的Cell尺寸，如果想要单独定义某个Cell的尺寸，可以使用下面方法：
- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout sizeForItemAtIndexPath:(NSIndexPath *)indexPath
```

- minimumLineSpacing属性(设定全局的行间距)

```objc
//设定全局的行间距，如果想要设定指定区内Cell的最小行距，可以使用下面方法：
- (CGFloat)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout *)collectionViewLayout minimumLineSpacingForSectionAtIndex:(NSInteger)section
```

- minimumInteritemSpacing属性 (设定全局的Cell间距)

```objc
//设定全局的Cell间距，如果想要设定指定区内Cell的最小间距，可以使用下面方法：

- (CGFloat)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout minimumInteritemSpacingForSectionAtIndex:(NSInteger)section;
```

- scrollDirection属性
- 设定滚动方向，有UICollectionViewScrollDirectionVertical和UICollectionViewScrollDirectionHorizontal两个值。

- headerReferenceSize属性与footerReferenceSize属性

```objc
//设定页眉和页脚的全局尺寸，需要注意的是，根据滚动方向不同，header和footer的width和height中只有一个会起作用。如果要单独设置指定区内的页面和页脚尺寸，可以使用下面方法：

- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout referenceSizeForHeaderInSection:(NSInteger)section

- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout referenceSizeForFooterInSection:(NSInteger)section
```

- sectionInset属性(设定全局的区内边距)

```objc
//设定全局的区内边距，如果想要设定指定区的内边距，可以使用下面方法：
- (UIEdgeInsets)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout insetForSectionAtIndex:(NSInteger)section;
```

####然后需要实现三种类型的委托：UICollectionViewDataSource, UICollectionViewDelagate和UICollectionViewDelegateFlowLayout。
```objc
@interface ViewController : UIViewController <UICollectionViewDelegateFlowLayout, UICollectionViewDataSource>
```
- 因为UICollectionViewDelegateFlowLayout实际上是UICollectionViewDelegate的一个子协议，它继承了UICollectionViewDelegate，所以只需要在声明处写上UICollectionViewDelegateFlowLayout就行了。


###下面是三个和高亮有关的方法：
```objc
- (BOOL)collectionView:(UICollectionView *)collectionView shouldHighlightItemAtIndexPath:(NSIndexPath *)indexPath

- (void)collectionView:(UICollectionView *)collectionView didHighlightItemAtIndexPath:(NSIndexPath *)indexPath

- (void)collectionView:(UICollectionView *)collectionView didUnhighlightItemAtIndexPath:(NSIndexPath *)indexPath
```


####事件的处理顺序如下：
- 手指按下
- shouldHighlightItemAtIndexPath (如果返回YES则向下执行，否则执行到这里为止)
- didHighlightItemAtIndexPath (高亮)
- 手指松开
- didUnhighlightItemAtIndexPath (取消高亮)
- shouldSelectItemAtIndexPath (如果返回YES则向下执行，否则执行到这里为止)
- didSelectItemAtIndexPath (执行选择事件)
- 如果只是简单实现点击后cell改变显示状态，只需要在cellForItemAtIndexPath方法里返回cell时，指定cell的selectedBackgroundView：

```objc
- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath
{
    UICollectionViewCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"myCell" forIndexPath:indexPath];

    UIView* selectedBGView = [[UIView alloc] initWithFrame:cell.bounds];
    selectedBGView.backgroundColor = [UIColor blueColor];
    cell.selectedBackgroundView = selectedBGView;

    return cell;
}
```

###如果要实现点击时(手指未松开)的显示状态与点击后(手指松开)的显示状态，则需要通过上面提到的方法来实现：
```objc
- (BOOL)collectionView:(UICollectionView *)collectionView shouldHighlightItemAtIndexPath:(NSIndexPath *)indexPath
{
    return YES;
}

- (void)collectionView:(UICollectionView *)colView didHighlightItemAtIndexPath:(NSIndexPath *)indexPath
{
    UICollectionViewCell* cell = [colView cellForItemAtIndexPath:indexPath];

    [cell setBackgroundColor:[UIColor purpleColor]];
}

- (void)collectionView:(UICollectionView *)colView  didUnhighlightItemAtIndexPath:(NSIndexPath *)indexPath
{
    UICollectionViewCell* cell = [colView cellForItemAtIndexPath:indexPath];

    [cell setBackgroundColor:[UIColor yellowColor]];
}
```
