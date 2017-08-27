#布局TabBar
##布局TabBar中的items

- Tabbar中应该有5个Item
    - 主页/消息/发布按钮/发现/我
- 布局TabBar中的Items可以通过下面的方式
    - 通过自定义TabBar的方式
    - 多添加一个控制器,让中间空出位置
- 注意:如果只是在storyboard中设置item的图片,不能设置选中的图片,因此需要通过代码重新设置

```swift
 /// 图片数组
    private lazy var imageNames : [String] = {
        return ["tabbar_home", "tabbar_message_center", "" ,"tabbar_discover", "tabbar_profile"]
    }()

    private func adjustItems() {
        for i in 0..<tabBar.items!.count {
            // 1.取出item
            let item = tabBar.items![i]

            // 2.如果是第二个item,则不可以和用户交互.并且不需要设置图片
            if i == 2 {
                item.enabled = false
                continue
            }

            // 2.设置图片
            item.image = UIImage(named: imageNames[i])
            item.selectedImage = UIImage(named: imageNames[i] + "_highlighted")
        }
    }
```

##添加'+'按钮

- 创建UIButton
    - 扩展UIButton的构造函数
```swift
    convenience init(imageName : String, bgImageName : String) {
        self.init()

        // 设置相关属性
        setBackgroundImage(UIImage(named: bgImageName), forState: .Normal)
        setBackgroundImage(UIImage(named: bgImageName + "_highlighted"), forState: .Highlighted)
        setImage(UIImage(named: imageName), forState: .Normal)
        setImage(UIImage(named: imageName + "_highlighted"), forState: .Highlighted)
        sizeToFit()
    }
```

### `通过懒加载创建UIButton,并且在viewWillAppear中添加按钮`
```swift
/// 发布微博按钮
    private lazy var composeBtn : UIButton = {
        // let btn = UIButton.createButton("tabbar_compose_icon_add", bgImageName: "tabbar_compose_button")
        let btn = UIButton(imageName: "tabbar_compose_icon_add", bgImageName: "tabbar_compose_button")
        btn.center = CGPoint(x: self.tabBar.bounds.width * 0.5, y: self.tabBar.bounds.height * 0.5)

        // 监听按钮的点击
        // Selector("composeBtnClick")
        // "composeBtnClick"
        btn.addTarget(self, action: Selector("composeBtnClick"), forControlEvents: .TouchUpInside)

        return btn
    }()

    override func viewWillAppear(animated: Bool) {
        super.viewWillAppear(animated)

        // 1.调整items
        adjustItems()

        // 2.添加`加号`按钮
        tabBar.addSubview(composeBtn)
    }
```

##如果监听事件，要加@objc
```swift
// MARK:- 时间监听
extension BaseViewController{

    @objc private func registerBtnClicl(){
        print("registerBtnClicl")
    }

     @objc private func loginBtnClick(){
          print("registerBtnClicl")
    }
}
```
