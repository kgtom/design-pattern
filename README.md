# design-pattern
总结一下工作中，常用的设计模式，温故而知新。

例如：订单模块

所有访问订单模块的数据，通过Facade模式,隐藏实现细节，提供API，保证业务高内聚。

订单模块访问其它模块数据，通过Proxy模式，不直接使用订单对象，保证业务低耦合。

给订单模板新增一个额外的功能，Decorator 动态地给一个对象添加一些额外的职责,遵循开闭原则，比增加子类更灵活。

给订单使用政策A，现在打算接入政策B，于是新增一个适配器类C，继承于A且初始化B的实例，让A与B关联起来。订单使用A就可以访问B了，遵循开闭原则。

总结：

Decorator 是新增行为，Proxy是控制访问行为，Adapter是转换行为，改变接口，Facade是一种简化接口行为。


