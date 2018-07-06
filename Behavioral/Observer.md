### 一、概念理解
Observer 观察者模式：一对多的关系，监视某个主题对象，当该主题对象改变时，所有观察者都收到消息并。

### 二、角色
 * 具体主题对象
 * 具体观察者对象
 * 抽象主题对象接口
 * 抽象观察者对象接口
 
 ### 三、案例分析
 
 演示例子：当机票订单状态为1时，分别发送通知给 供应商、op、销售、旅客发送短信。
 
~~~go
/*

Observer 观察者模式：一对多的关系，监视某个主题对象，当该主题对象改变时，所有观察者都收到消息并。

思路：一个主题对象，多个观察者对象；

 传统做法，两个对象耦合性太强，不推荐使用：观察者对象作为主题对象结构体中属性，当主题对象改变时，观察者对象调用自己update发送消息。
 推荐：基于面向接口编程的原则，两个对象抽象各自接口，然后基于接口去调用。
 
*/

package main

import (
	"fmt"
	"time"
)

//定义一个事件，该事件发生变化时通知所有的观察者
//当订单状态为[已预订 即：state=1时通知观察者]
type OrderEvent struct {
	Id        int64
	passenger string //包含passenger 姓名及手机号

	State int
}

//定义观察者对象的接口
type OrderObserver interface {
	//定义一个更新发生事件的方法
	SengMsg(*OrderEvent)
}

//定义被观察者对象的接口(即：主题对象接口)
type Subject interface {
	//注册观察者
	Regist(OrderObserver)
	//注销观察者
	Deregist(OrderObserver)
	//状态改变
	SetState(*OrderEvent)
	//通知观察者事件
	Notify(*OrderEvent)
}

//具体观察者
type ConcreteObserve struct {
	Id int
}

//具体观察者对象实现 Observer 接口
func (co *ConcreteObserve) SengMsg(e *OrderEvent) {
	fmt.Printf("observer [%d] ,receive msg,orderId: %d,passenger:%s  send msg \n", co.Id, e.Id, e.passenger)
}

//具体被观察者(即：主题对象)
//所有观察者对象的引用保存在一个map中，并提供增加和删除观察者对象的操作
type ConcreteSubject struct {
	observers map[OrderObserver]struct{}
}

//具体被观察者对象实现 Subject接口
func (cs *ConcreteSubject) Regist(ob OrderObserver) {
	cs.observers[ob] = struct{}{}
}
func (cs *ConcreteSubject) Deregist(ob OrderObserver) {
	delete(cs.observers, ob)
}
func (cs *ConcreteSubject) Notify(e *OrderEvent) {

	for ob, _ := range cs.observers {
		ob.SengMsg(e)
	}
}
func (cs *ConcreteSubject) SetState(e *OrderEvent) {
	e.State = 1
}
func main() {
	//实例化被观察者
	cs := &ConcreteSubject{
		observers: make(map[OrderObserver]struct{}),
	}
	//实例化观察者
	cobserverPassenger := &ConcreteObserve{Id: 1}
	cobserverOp := &ConcreteObserve{Id: 2}
	cobserverSaler := &ConcreteObserve{Id: 3}
	cobserverSupplier := &ConcreteObserve{Id: 4}

	//注册观察者
	cs.Regist(cobserverPassenger)
	cs.Regist(cobserverOp)
	cs.Regist(cobserverSaler)
	cs.Regist(cobserverSupplier)

	//模拟订单状态改变为 1，此时需要给观察者发送消息
	cs.SetState(&OrderEvent{State: 1})

	//方一

	e := &OrderEvent{
		Id:        time.Now().UnixNano(),
		passenger: "tom",
	}
	cs.Notify(e)

	//方二
	//	stop := time.NewTimer(3 * time.Second).C
	//	tick := time.NewTicker(1 * time.Second).C
	//	for {
	//		select {
	//		case <-stop:
	//			goto Loop
	//		case t := <-tick:
	//			cs.Notify(&OrderEvent{
	//				Id:        t.UnixNano(),
	//				passenger: "tom",
	//				State:     1,
	//			})
	//		}
	//	}
	//Loop:
	fmt.Println("end")
}


~~~
