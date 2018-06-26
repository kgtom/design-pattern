---


---

<p><strong>概念理解：</strong></p>
<blockquote>
<p>命令模式是将一个请求封装为一个对象,从而使我们可用不同的请求对客户进行参数化;对请求排队或者记录请求日志,以及支持可撤销的操作.</p>
</blockquote>
<blockquote>
<p>命令模式是一种对象行为型模式</p>
</blockquote>
<p>在命令模式中有如下几个角色:</p>
<ul>
<li>Command: 命令</li>
<li>Invoker: 调用者</li>
<li>Receiver: 接受者</li>
<li>Client: 客户端</li>
</ul>
<p>他们的关系可以这么来描述:</p>
<blockquote>
<p>客户端通过调用者发送命令,命令调用接收者执行相应操作.</p>
</blockquote>
<p>其实命令模式也很简单,不过不知道大家发现没有,在上述描述中调用者和接收者并不知道对方的存在,也就是说他们之间是解耦合的.</p>
<p><strong>举例：</strong><br>
用遥控器的例子来解释一下吧,<br>
遥控器对应上面的角色就是调用者,<br>
电视就是接收者,<br>
命令呢?对应的就是遥控器上的按键,<br>
客户端就是我们人,</p>
<p><strong>流程：</strong><br>
当我们想打开电视的时候,就会通过遥控器(调用者)的遥控器按键(命令)来打开/关闭电视(接收者),在这个过程中遥控器是不知道电视的,但是电源按钮是知道他要控制谁的什么操作.</p>
<p>人(客户端)----&gt;调用者(遥控器)—&gt;发送命令(遥控器按键)—&gt;命令接受执行操作(电视)----</p>
<p><strong>优点</strong></p>
<ul>
<li>解耦:降低各个模块的耦合性</li>
<li>遵循开闭原则：扩展命令，不需要修改代码</li>
</ul>
<p><strong>代码</strong></p>
~~~go
package main

import "fmt"

//receiver 命令接受者
type TV struct {
}

func (p TV) Open() {
	fmt.Println("tv open")
}
func (p TV) Close() {
	fmt.Println("tv close")
}

//Command 命令
type Command interface {
	Press()
}

type OpenCommand struct {
	tv TV
}

func (c OpenCommand) Press() {
	c.tv.Open()
}

type CloseCommand struct {
	tv TV
}

func (c CloseCommand) Press() {

	c.tv.Close()
}

//sender 发送命令
type Invoker struct {
	cmd Command
}

func (i *Invoker) SetCommand(cmd Command) {
	i.cmd = cmd
}
func (i Invoker) Do() {
	i.cmd.Press()
}


func NewCommand() *CommandArray {
	c := &CommandArray{}
	c.cmds = make([]Command, 2)
	return c
}

func main() {

	////命令模式----单命令
	//Client

	var tv TV
	//第一种初始化
	//s := Invoker{OpenCommand{tv}}
	//第二种初始
	s := Invoker{}
	s.SetCommand(OpenCommand{tv})
	s.Do()

	s.SetCommand(CloseCommand{tv})
	s.Do()

	
}


~~~

<p><strong>多个命令模式：组合命令</strong></p>
~~~go
//命令集合
type CommandArray struct {
	Index int
	cmds  []Command
}

func (c *CommandArray) AddCommandArray(cmd Command) {

	c.cmds[c.Index] = cmd
	c.Index++
}
func (c *CommandArray) Press() {

	for _, v := range c.cmds {
		v.Press()
	}
}

func NewCommand() *CommandArray {
	c := &CommandArray{}
	c.cmds = make([]Command, 2)
	return c
}

func main() {

	

	//命令模式----组合命令
	fmt.Println("-------组合命令-------")
	s1 := NewCommand()
	s1.AddCommandArray(OpenCommand{tv})
	s1.AddCommandArray(CloseCommand{tv})
	s1.Press()
}
~~~

<blockquote>
<p>Reference:<br>
<a href="https://blog.csdn.net/qibin0506/article/details/50812611">addr</a></p>
</blockquote>

