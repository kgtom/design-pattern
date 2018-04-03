# design-pattern
总结一下工作中，常用的设计模式，温故而知新。

例如：订单模块

所有访问订单模块的数据，通过Facade模式,隐藏实现细节，提供API，保证业务高内聚。

订单模块访问其它模块数据，通过Proxy模式，不直接使用订单对象，保证业务低耦合。

给订单模板新增一个额外的功能，Decorator动态地给一个对象添加一些额外的职责,遵循开闭原则，比增加子类更灵活。
