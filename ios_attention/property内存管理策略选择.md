# @property内存管理策略选择
- 非ARC
    + 1> copy : 只用于NSString\block
    + 2> retain : 除NSString\block以外的OC对象
    + 3> assign :基本数据类型、枚举、结构体(非OC对象),当2个对象相互引用,一端用retain,一端用assign

- ARC
    + 1> copy : 只用于NSString\block
    + 2> strong : 除NSString\block以外的OC对象
    + 3> weak : 当2个对象相互引用,一端用strong,一端用weak
    + 4> assgin : 基本数据类型、枚举、结构体(非OC对象)
---
