#swift类方法
```swift
 class func vistorView() -> VistorView{
        return NSBundle.mainBundle().loadNibNamed("VistorView", owner: nil, options: nil).first as! VistorView
    }
```
