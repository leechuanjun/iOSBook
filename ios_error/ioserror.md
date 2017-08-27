1、UITableView 的cell忘记注册

Assertion failure  in-[UITableView_configureCellForDisplay:forIndexPath:], /BuildRoot/Library/Caches/com.apple.xbs/Sources/UIKit_Sim/UIKit-3599.6/UITableView.m:8035

解决办法：cell注册
```objc
[self.tableView registerClass:[ZJCTableViewSettingBaseCell class] forCellReuseIdentifier:ID];
```
