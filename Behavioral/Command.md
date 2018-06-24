---


---

<h2 id="概念">概念</h2>
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
<p>用遥控器的例子来解释一下吧,<br>
遥控器对应上面的角色就是调用者,<br>
电视就是接收者,<br>
命令呢?对应的就是遥控器上的按键,<br>
客户端就是我们人,</p>
<p>当我们想打开电视的时候,就会通过遥控器(调用者)的电源按钮(命令)来打开电视(接收者),在这个过程中遥控器是不知道电视的,但是电源按钮是知道他要控制谁的什么操作.</p>
<pre><code>

</code></pre>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

