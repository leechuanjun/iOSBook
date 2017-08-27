##IOS 面试题
##1.isa指针
- 每一个对象都包含一个isa指针.这个指针指向当前对象所属的类。
- [p eat];表示给p所指向的对象发送一条eat消息,调用对象的eat方法,此时对象会顺着内部的isa指针找到存 储于类中的方法,执行。
- isa是对象中的隐藏指针,指向创建这个对象的类。
- 通过isa指针我们可以在运行的时候知道当前对象是属于那个Class（类）的
![](../images/oc/isazz.png)


##2.\#import和\#include区别
- \#import与\#include的类似，都是把其后面的文件拷贝到该指令所在的地方
- \#import可以自动防止重复导入
- \#import <> 用于包含系统文件
- \#import""用于包含本项目中的文件

- \#import和@class作用上的区别
    + \#import会包含引用类的所有信息(内容),包括引用类的变量和方法
    + @class仅仅是告诉编译器有这么一个类, 具体这个类里有什么信息, 完全不知

##为什么 NotificationCenter 要 removeObserver? 如何实现自动 remove?
- 如果不移除的话,万一注册通知的类被销毁以后又发了通知,程序会崩溃.因为向野指针发送了消息
- 实现自动remove:通过自释放机制,通过动态属性将remove转移给第三者,解除耦合,达到自动实现remove。

##4.Push Notification 是如何工作的？
- 推送通知分为两种,一个是本地推送,一个是远程推送
- 本地推送：不需要联网也可以推送,是开发人员在APP内设定特定的时间来提醒用户干什么

- 远程推送：需要联网,用户的设备会于苹果APNS服务器形成一个长连接,用户设备会发送UUID 和Bundle idenidentifier给苹果服务器,苹果服务器会加密生成一个deviceToken给用户设备,然后设备会将deviceToken发送给APP的服务器,服务器会将deviceToken存进他们的数据库,这时候如果有人发送消息给我,服务器端就会去查询我的deviceToken,然后将deviceToken和要发送的信息发送给苹果服务器,苹果服务器通过deviceToken找到我的设备并将消息推送到我的设备上,这里还有个情况是如果APP在线,那么APP服务器会于APP产生一个长连接,这时候APP服务器会直接通过deviceToken将推送到设备上。


# 模式
- 单例模式

```objc
+ (instancetype)allocWithZone:(struct _NSZone *)zone

{
    static XMGPerson *_person = nil;

    static dispatch_once_t onceToken;

    dispatch_once(&onceToken, ^{

        _person = [super allocWithZone:zone];

    });

    return _person;

}
```

5##runtime

### 一、runtime简介
*	RunTime简称运行时。OC就是`运行时机制`，也就是在运行时候的一些机制，其中最主要的是消息机制。
*	对于C语言，`函数的调用在编译的时候会决定调用哪个函数`。
*	对于OC的函数，属于`动态调用过程`，在编译的时候并不能决定真正调用哪个函数，只有在真正运行的时候才会根据函数的名称找到对应的函数来调用。
*	事实证明：
	*	在编译阶段，OC可以`调用任何函数`，即使这个函数并未实现，只要声明过就不会报错。
	*	在编译阶段，C语言调用`未实现的函数`就会报错。

####方法调用流程
-  怎么去调用eat方法 ,对象方法:类对象的方法列表 类方法:元类中方法列表
- 1.通过isa去对应的类中查找
- 2.注册方法编号
- 3.根据方法编号去查找对应方法
- 4.找到只是最终函数实现地址,根据地址去方法区调用对应函数

  Runtime(动态添加方法):OC都是懒加载机制,只要一个方法实现了,就会马上添加到方法列表中.
    app:免费版,收费版
    QQ,微博,直播等等应用,都有会员机制

## 美团有个面试题?有没有使用过performSelector,什么时候使用?动态添加方法的时候使用过?怎么动态添加方法?用runtime?为什么要动态添加方法?
- 答：动态添加方法的时候用performSelector，用runtime来动态添加方法，为什么要动态添加方法，OC都是懒加载机制,只要一个方法实现了,就会马上添加到方法列表中，例如：app:免费版,收费版，Q,微博,直播等等应用,都有会员机制；




