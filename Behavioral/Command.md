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
<pre class=" language-go"><code class="prism  language-go"><span class="token keyword">package</span> main

<span class="token keyword">import</span> <span class="token string">"fmt"</span>

<span class="token comment">//receiver 命令接受者</span>
<span class="token keyword">type</span> TV <span class="token keyword">struct</span> <span class="token punctuation">{</span>
<span class="token punctuation">}</span>

<span class="token keyword">func</span> <span class="token punctuation">(</span>p TV<span class="token punctuation">)</span> <span class="token function">Open</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
	fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"tv open"</span><span class="token punctuation">)</span>
<span class="token punctuation">}</span>
<span class="token keyword">func</span> <span class="token punctuation">(</span>p TV<span class="token punctuation">)</span> <span class="token function">Close</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
	fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"tv close"</span><span class="token punctuation">)</span>
<span class="token punctuation">}</span>

<span class="token comment">//Command 命令</span>
<span class="token keyword">type</span> Command <span class="token keyword">interface</span> <span class="token punctuation">{</span>
	<span class="token function">Press</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
<span class="token punctuation">}</span>

<span class="token keyword">type</span> OpenCommand <span class="token keyword">struct</span> <span class="token punctuation">{</span>
	tv TV
<span class="token punctuation">}</span>

<span class="token keyword">func</span> <span class="token punctuation">(</span>c OpenCommand<span class="token punctuation">)</span> <span class="token function">Press</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
	c<span class="token punctuation">.</span>tv<span class="token punctuation">.</span><span class="token function">Open</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
<span class="token punctuation">}</span>

<span class="token keyword">type</span> CloseCommand <span class="token keyword">struct</span> <span class="token punctuation">{</span>
	tv TV
<span class="token punctuation">}</span>

<span class="token keyword">func</span> <span class="token punctuation">(</span>c CloseCommand<span class="token punctuation">)</span> <span class="token function">Press</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

	c<span class="token punctuation">.</span>tv<span class="token punctuation">.</span><span class="token function">Close</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
<span class="token punctuation">}</span>

<span class="token comment">//sender 发送命令</span>
<span class="token keyword">type</span> Invoker <span class="token keyword">struct</span> <span class="token punctuation">{</span>
	cmd Command
<span class="token punctuation">}</span>

<span class="token keyword">func</span> <span class="token punctuation">(</span>i <span class="token operator">*</span>Invoker<span class="token punctuation">)</span> <span class="token function">SetCommand</span><span class="token punctuation">(</span>cmd Command<span class="token punctuation">)</span> <span class="token punctuation">{</span>
	i<span class="token punctuation">.</span>cmd <span class="token operator">=</span> cmd
<span class="token punctuation">}</span>
<span class="token keyword">func</span> <span class="token punctuation">(</span>i Invoker<span class="token punctuation">)</span> <span class="token function">Do</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
	i<span class="token punctuation">.</span>cmd<span class="token punctuation">.</span><span class="token function">Press</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
<span class="token punctuation">}</span>

<span class="token keyword">func</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

	<span class="token comment">//Client</span>

	<span class="token keyword">var</span> tv TV
	<span class="token comment">//第一种初始化</span>
	<span class="token comment">//s := Invoker{OpenCommand{tv}}</span>
	<span class="token comment">//第二种初始</span>
	s <span class="token operator">:=</span> Invoker<span class="token punctuation">{</span><span class="token punctuation">}</span>
	s<span class="token punctuation">.</span><span class="token function">SetCommand</span><span class="token punctuation">(</span>OpenCommand<span class="token punctuation">{</span>tv<span class="token punctuation">}</span><span class="token punctuation">)</span>
	s<span class="token punctuation">.</span><span class="token function">Do</span><span class="token punctuation">(</span><span class="token punctuation">)</span>

	s<span class="token punctuation">.</span><span class="token function">SetCommand</span><span class="token punctuation">(</span>CloseCommand<span class="token punctuation">{</span>tv<span class="token punctuation">}</span><span class="token punctuation">)</span>
	s<span class="token punctuation">.</span><span class="token function">Do</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
<span class="token punctuation">}</span>


</code></pre>
<p><strong>多个命令模式：组合命令</strong></p>
<pre class=" language-go"><code class="prism  language-go"><span class="token comment">//命令集合</span>
<span class="token keyword">type</span> CommandArray <span class="token keyword">struct</span> <span class="token punctuation">{</span>
	Index <span class="token builtin">int</span>
	cmds  <span class="token punctuation">[</span><span class="token punctuation">]</span>Command
	<span class="token comment">//用一个切片来保存各个命令</span>
<span class="token punctuation">}</span>

<span class="token keyword">func</span> <span class="token punctuation">(</span>c <span class="token operator">*</span>CommandArray<span class="token punctuation">)</span> <span class="token function">AddCommandArray</span><span class="token punctuation">(</span>cmd Command<span class="token punctuation">)</span> <span class="token punctuation">{</span>

	c<span class="token punctuation">.</span>cmds<span class="token punctuation">[</span>c<span class="token punctuation">.</span>Index<span class="token punctuation">]</span> <span class="token operator">=</span> cmd
	c<span class="token punctuation">.</span>Index<span class="token operator">++</span>
<span class="token punctuation">}</span>
<span class="token keyword">func</span> <span class="token punctuation">(</span>c <span class="token operator">*</span>CommandArray<span class="token punctuation">)</span> <span class="token function">Press</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

	<span class="token keyword">for</span> <span class="token boolean">_</span><span class="token punctuation">,</span> v <span class="token operator">:=</span> <span class="token keyword">range</span> c<span class="token punctuation">.</span>cmds <span class="token punctuation">{</span>
		v<span class="token punctuation">.</span><span class="token function">Press</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
	<span class="token punctuation">}</span>
<span class="token punctuation">}</span>

<span class="token keyword">func</span> <span class="token function">NewCommand</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">*</span>CommandArray <span class="token punctuation">{</span>
	c <span class="token operator">:=</span> <span class="token operator">&amp;</span>CommandArray<span class="token punctuation">{</span><span class="token punctuation">}</span>
	c<span class="token punctuation">.</span>cmds <span class="token operator">=</span> <span class="token function">make</span><span class="token punctuation">(</span><span class="token punctuation">[</span><span class="token punctuation">]</span>Command<span class="token punctuation">,</span> <span class="token number">2</span><span class="token punctuation">)</span>
	<span class="token keyword">return</span> c
<span class="token punctuation">}</span>

<span class="token keyword">func</span> <span class="token function">main</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>

	<span class="token comment">////命令模式----单命令</span>
	<span class="token comment">//Client</span>

	<span class="token keyword">var</span> tv TV
	<span class="token comment">//第一种初始化</span>
	<span class="token comment">//s := Invoker{OpenCommand{tv}}</span>
	<span class="token comment">//第二种初始</span>
	s <span class="token operator">:=</span> Invoker<span class="token punctuation">{</span><span class="token punctuation">}</span>
	s<span class="token punctuation">.</span><span class="token function">SetCommand</span><span class="token punctuation">(</span>OpenCommand<span class="token punctuation">{</span>tv<span class="token punctuation">}</span><span class="token punctuation">)</span>
	s<span class="token punctuation">.</span><span class="token function">Do</span><span class="token punctuation">(</span><span class="token punctuation">)</span>

	s<span class="token punctuation">.</span><span class="token function">SetCommand</span><span class="token punctuation">(</span>CloseCommand<span class="token punctuation">{</span>tv<span class="token punctuation">}</span><span class="token punctuation">)</span>
	s<span class="token punctuation">.</span><span class="token function">Do</span><span class="token punctuation">(</span><span class="token punctuation">)</span>

	<span class="token comment">////命令模式----组合命令</span>
	fmt<span class="token punctuation">.</span><span class="token function">Println</span><span class="token punctuation">(</span><span class="token string">"-------组合命令-------"</span><span class="token punctuation">)</span>
	s1 <span class="token operator">:=</span> <span class="token function">NewCommand</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
	s1<span class="token punctuation">.</span><span class="token function">AddCommandArray</span><span class="token punctuation">(</span>OpenCommand<span class="token punctuation">{</span>tv<span class="token punctuation">}</span><span class="token punctuation">)</span>
	s1<span class="token punctuation">.</span><span class="token function">AddCommandArray</span><span class="token punctuation">(</span>CloseCommand<span class="token punctuation">{</span>tv<span class="token punctuation">}</span><span class="token punctuation">)</span>
	s1<span class="token punctuation">.</span><span class="token function">Press</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
<span class="token punctuation">}</span>

</code></pre>
<blockquote>
<p>Reference:<br>
<a href="https://blog.csdn.net/qibin0506/article/details/50812611">addr</a></p>
</blockquote>

