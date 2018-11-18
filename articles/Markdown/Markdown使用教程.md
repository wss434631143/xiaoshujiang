<html>

<div id="cnblogs_post_body" class="blogpost-body cnblogs-markdown"><h2 id="前言">前言</h2>
<p>　　以前经常在 <strong>github</strong> 中看到 <em>.md</em> 格式的文件，一直没有注意，也不明白为什么文本文档的后缀不是 <em>.txt</em> ,后来无意中看到了 <strong>Markdown</strong>，看到了用这个东西写得一些web界面等特别的规整漂亮，顿时不明觉厉。后来自己学习了一下，感觉这个语言确实简洁、美观，现推荐出来供大家学习和玩玩，希望能对你有用。</p>
<p>　　本文<strong>图文并茂</strong>，避免了单纯看语法的枯燥和繁琐，其实，学习一门新东西真的其乐无穷！</p>
<h2 id="简介">简介　　</h2>
<p>Markdown 是一种用来文本处理的轻量级 <strong>「标记语言」</strong>，它用简洁的语法代替排版，而无需像Microsoft的Word一样需要花费大量的时间进行排版、字体设置。它使我们专心于码字，用「标记」语法，来代替常见的排版格式。Markdown不止可以处理文本，使得文字更美观，还支持图像、表格等的插入，大大方便了我们的写作。例如此文从内容到格式，甚至插图，一个键盘可以搞定了，无需鼠标！</p>
<p>　　目前来看，支持 Markdown 语法的编辑器有很多，包括很多网站（例如简书）也支持了 Markdown 的文字录入。Markdown 从写作到完成，导出格式随心所欲，你可以导出 HTML 格式的文件用来网站发布，也可以十分方便的导出 PDF 格式，甚至可以利用 CloudApp 这种云服务工具直接上传至网页用来分享你的文章，全球最大的轻博客平台 Tumblr，也支持 Mou 这类 Markdown 工具的直接上传。</p>
<p>　　目前，我们的 <strong>博客园</strong> 同样支持了Markdown文本的编辑，具体如何设置请往下看！</p>
<h2 id="markdown特点">Markdown特点</h2>
<ul>
<li>专注你的文字内容而不是排版样式；</li>
<li>轻松的导出 HTML、PDF 和本身的 .md 文件；</li>
<li>纯文本内容，兼容所有的文本编辑器与字处理软件；</li>
<li>可读，直观。适合所有人的写作语言。</li>
</ul>
<h2 id="教程">教程</h2>
<blockquote>
<h3 id="简明教程">简明教程：</h3>
<ul>
<li><a href="http://wowubuntu.com/markdown/basic.html" class="uri">http://wowubuntu.com/markdown/basic.html</a></li>
</ul>
<h3 id="详细教程">详细教程</h3>
<ul>
<li><a href="http://wowubuntu.com/markdown/index.html" class="uri">http://wowubuntu.com/markdown/index.html</a></li>
</ul>
</blockquote>
<h2 id="博客园配置markdown编辑器">博客园配置Markdown编辑器</h2>
<ol>
<li>进入博客后台<br>
</li>
<li>点击“设置默认编辑器”</li>
<li>选中 <strong>Markdown</strong>并保存</li>
<li>回到随笔界面点击“添加随笔”</li>
<li>在“Markdown编辑器”中输入相应的代码<br>
<img src="http://ory0rmuh5.bkt.clouddn.com/blog/170727/fkjb2lAb2h.png?imageslim" alt="第一步"><br>
<img src="http://ory0rmuh5.bkt.clouddn.com/blog/170727/j7D3FD9d0b.png?imageslim" alt="第二步"></li>
</ol>
<hr>
<h2 id="语法">语法</h2>
<h3 id="标题">1. 标题</h3>
<p>标题通过 <strong><code>#</code></strong> 的个数来进行区分，Mardown总共支持6级标题。<br>
<img src="http://ww1.sinaimg.cn/large/6aee7dbbgw1effeaclhiyj20eh09cwez.jpg" alt="标题"></p>
<h3 id="段落-换行">2. 段落 &amp; 换行</h3>
<h4 id="首行缩进空格">2.1. 首行缩进/空格:</h4>
<blockquote>
<ul>
<li><code>&amp;nbsp;</code>: 英文空格(半角)<br>
</li>
<li><code>&amp;emsp;</code>: 中文空格(全角)<br>
</li>
<li>输入法切换至全角，双击 <strong>空格</strong> <em>「推荐」</em></li>
<li>半方大的空白 <code>&amp;ensp;</code> 或 <strong>&amp;#8194</strong></li>
<li>全方大的空白<code>&amp;emsp;</code> 或 <strong>&amp;#8195</strong></li>
<li>不断行的空白格 <code>&amp;nbsp;</code> 或 <strong>&amp;#160</strong></li>
</ul>
</blockquote>
<h4 id="强制换行">2.2. 强制换行</h4>
<p>　　连续的字符串，如果你想要换行，往往打“Enter”是不管用的，正确的换行方法为在 <strong>「在需要换行的地方插入 &gt;=2 个 空格」</strong></p>
<h4 id="空行">2.3. 空行</h4>
<p>两种方式：</p>
<blockquote>
<ul>
<li>在markdown中加入 <strong>&gt;=2 个空行</strong>.</li>
<li>使用<code>&lt;br&gt;</code> <strong><em>【推荐】</em></strong><br>
　　<br>
<img src="http://ory0rmuh5.bkt.clouddn.com/blog/170625/32Klm6Jdk6.png?imageslim" alt="mark"></li>
</ul>
</blockquote>
<h3 id="列表">3. 列表</h3>
<p>在Markdown下，有四种列表：有序和无序；</p>
<blockquote>
<ul>
<li>有序列表：采用 <code>1.</code> <code>2.</code> <code>3.</code>的形式</li>
<li>无序列表：采用前面加 <code>*</code> <code>-</code> <code>+</code> 的方式，支持多级嵌套<br>
</li>
<li>未完成列表：<code>- [ ]</code>，每个符号间均有空格</li>
<li>已完成列表：<code>- [x]</code>，注意空格使用<br>
<strong>PS: 符号与文字之间必须有 <em>空格</em></strong></li>
</ul>
</blockquote>
<p><img src="http://ory0rmuh5.bkt.clouddn.com/blog/170625/mCJKfdfa8K.png?imageslim" alt="mark"><br>
<img src="http://ory0rmuh5.bkt.clouddn.com/blog/170626/FI7K6Ii91D.png?imageslim" alt="mark"></p>
<h3 id="引用-quote">4. 引用 (Quote)</h3>
<p>若需要引入有出处的一段话等，可以采用<strong>引用</strong>的方式实现，实现方式为在<strong>行开始处</strong>加入<code>&gt;</code>,如下所示：<br>
<img src="http://ory0rmuh5.bkt.clouddn.com/blog/170625/d43BjccFEF.png?imageslim" alt="mark"></p>
<h3 id="字体设置">5. 字体设置</h3>
<blockquote>
<ul>
<li><strong>粗体</strong>：<br>
字符串前后均加上 <code>**</code></li>
<li><em>斜体</em><br>
字符串前后均加上 <code>*</code><br>
</li>
<li><del>删除线</del><br>
字符串前后各加 <code>~~</code><br>
</li>
<li>++下划线++<br>
字符串前后各加 <code>++</code><br>
</li>
<li>== 字体背景色 ==<br>
字符串前后各加 <code>==</code><br>
</li>
<li><code>标记</code><br>
字体前后加上 ` (Esc下方的那个键)</li>
</ul>
</blockquote>
<p><img src="http://ory0rmuh5.bkt.clouddn.com/blog/170626/A8eFff93B8.png?imageslim" alt="mark"></p>
<h3 id="分割带">6. 分割带</h3>
<p>当上下文不属于同一模块或者无甚关联时刻，可以使用分隔符进行隔开；分隔符的格式如下：</p>
<blockquote>
<ul>
<li>连续多个<code>-</code>(<strong>&gt;=3</strong>)</li>
<li>连续多个<code>*</code>(<strong>&gt;=3</strong>)<br>
</li>
<li>连续多个下划线 <code>_</code> (<strong>&gt;=3</strong>)</li>
<li><strong>PS：以上，分隔符中间可以有空格，但分割行不可有其它字符存在</strong></li>
</ul>
</blockquote>
<p><img src="http://ory0rmuh5.bkt.clouddn.com/blog/170625/jDCiIafkKc.png?imageslim" alt="mark"></p>
<h3 id="图片和链接">7. 图片和链接</h3>
<blockquote>
<ul>
<li><strong>图片</strong><br>
<code>![]()</code> : [图片名称] (图片网络地址)</li>
<li><strong>链接</strong><br>
<code>[]()</code> ： [链接名称(可自定义)] (链接地址)</li>
</ul>
</blockquote>
<p><img src="http://ww2.sinaimg.cn/large/6aee7dbbgw1efffa67voyj20ix0ctq3n.jpg"></p>
<h3 id="代码块">8. 代码块</h3>
<p>和程序相关的写作或是标签语言原始码通常会有已经排版好的代码区块，通常这些区块我们并不希望它以一般段落文件的方式去排版，而是照原来的样子显示，Markdown 会用<code>制表符</code>来将代码包起来。<br>
代码块一直持续到没有缩进的那一行(或是文件的结尾)</p>
<div class="sourceCode"><pre class="sourceCode cpp"><code class="sourceCode cpp hljs">    <span class="ot"><span class="hljs-meta">#<span class="hljs-meta-keyword">include</span> <span class="hljs-meta-string">&lt;iostrem&gt;</span>  </span></span>
    <span class="kw"><span class="hljs-keyword">using</span></span> <span class="kw"><span class="hljs-keyword">namespace</span></span> <span class="hljs-built_in">std</span>;  
    <span class="dt"><span class="hljs-function"><span class="hljs-keyword">int</span></span></span><span class="hljs-function"> <span class="hljs-title">main</span><span class="hljs-params">(</span></span><span class="dt"><span class="hljs-function"><span class="hljs-params"><span class="hljs-keyword">int</span></span></span></span><span class="hljs-function"><span class="hljs-params"> argc, </span></span><span class="dt"><span class="hljs-function"><span class="hljs-params"><span class="hljs-keyword">char</span></span></span></span><span class="hljs-function"><span class="hljs-params"> **argv)</span>  
    </span>{
        <span class="hljs-built_in">cout</span> &lt;&lt; <span class="st"><span class="hljs-string">"hello,world!"</span></span>;
    }</code></pre></div>
<p><img src="http://ory0rmuh5.bkt.clouddn.com/blog/170625/90icD8L58k.png?imageslim" alt="mark"></p>
<h3 id="网址自动转换">9. 网址自动转换</h3>
<p>Markdown 支持以比较简短的自动链接形式来处理网址和电子邮件信箱，只要是<code>&lt;&gt;</code>包起来， Markdown 就会自动把它转成链接。一般网址的链接文字就和链接地址一样，例如：</p>
<blockquote>
<p>这个是我的博客地址：<a href="https://home.cnblogs.com/u/Jimmy1988/" class="uri">https://home.cnblogs.com/u/Jimmy1988/</a><br>
我的邮箱地址是：<a href="mailto:JimmyNie2017@163.com">JimmyNie2017@163.com</a></p>
</blockquote>
<p><img src="http://ory0rmuh5.bkt.clouddn.com/blog/170625/3b2dCf9iBK.png?imageslim" alt="mark"></p>
<h3 id="转义">10. 转义</h3>
<p>Markdown 可以利用反斜杠来插入一些在语法中有其它意义的符号，例如：如果你想要用星号加在文字旁边的方式来做出强调效果，你可以在星号的前面加上反斜杠：</p>
<blockquote>
<p>*literal asterisks*</p>
</blockquote>
<p>Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：</p>
<blockquote>
<p>\ 反斜线<br>
` 反引号<br>
* 星号<br>
_ 下划线<br>
{} 花括号<br>
[] 方括号<br>
() 括弧<br>
# 井字号<br>
+ 加号<br>
- 减号<br>
. 英文句点<br>
! 感叹号</p>
</blockquote>
<h3 id="生成目录">11. 生成目录</h3>
<ul>
<li><p>前提条件：</p>
<blockquote>
<p>标题的建立是采用MD格式实现的，目录的生成建议放在文本最开始部分（当然也可以嵌入在文中）。</p>
</blockquote></li>
<li><p>语法</p>
<blockquote>
<p><code>[TOC]</code>，中间不要有空格</p>
</blockquote></li>
<li><p>注意事项</p>
<blockquote>
<p>目录的生成并不是每个编辑器都支持的，至今我用过的编辑器 <strong>有道云笔记</strong>是支持的。</p>
</blockquote></li>
</ul>
<p><img src="http://ory0rmuh5.bkt.clouddn.com/blog/170626/mFIgL02cl1.png?imageslim" alt="mark"></p>
<h3 id="表格">12. 表格</h3>
<p><img src="http://ory0rmuh5.bkt.clouddn.com/blog/171108/IkBLdlif2I.png?imageslim" alt="mark"><br>
表格的做法通常为：</p>
<blockquote>
<p>header 1 | header 2<br>
--- |---<br>
row 1 col 1 | row 1 col 2<br>
row 2 col 1 | row 2 col 2</p>
</blockquote>
<p>可用`&lt;br&gt;进行单元格内换行； 但是暂时不支持合并单元格</p>
<hr>
<h2 id="工具推荐">工具推荐</h2>
<blockquote>
<p><strong>windows平台</strong></p>
<ul>
<li><a href="http://markdownpad.com/">Markdown Pad</a></li>
<li><a href="http://code52.org/DownmarkerWPF/">Markpad</a></li>
</ul>
</blockquote>
<blockquote>
<p><strong>Linux平台</strong></p>
<ul>
<li><a href="http://sourceforge.net/p/retext/home/ReText/">ReText</a></li>
</ul>
</blockquote>
<blockquote>
<p><strong>Mac平台</strong></p>
<ul>
<li><a href="http://markable.in/">Markable.in</a></li>
<li><a href="http://dillinger.io/">Dillinger.io</a></li>
</ul>
</blockquote>
<blockquote>
<p><strong>浏览器插件</strong></p>
<ul>
<li><a href="https://chrome.google.com/webstore/detail/oknndfeeopgpibecfjljjfanledpbkog">MaDe(chrome)</a></li>
</ul>
</blockquote>
<blockquote>
<p><strong>在线编辑器</strong></p>
<ul>
<li><a href="http://mahua.jser.me/">麻花</a>：支持在线编辑，关键支持<code>VIM</code>命令</li>
</ul>
</blockquote>
<blockquote>
<p><strong>高级应用</strong></p>
<ul>
<li><a href="http://www.sublimetext.com/3">Sublime Text3</a> + <a href="http://ttscoff.github.io/MarkdownEditing/">Markdown Editor</a> + <a href="http://lucifr.com/2012/07/12/markdownediting-for-sublime-text-2/">教程</a></li>
</ul>
</blockquote>
<blockquote>
<p><strong>更多工具</strong></p>
<ul>
<li><a href="http://www.williamlong.info/archives/4319.html" class="uri">http://www.williamlong.info/archives/4319.html</a></li>
</ul>
</blockquote>
<hr>
<p><img src="http://ory0rmuh5.bkt.clouddn.com/blog/170624/eKHFmk26L6.png?imageslim" alt="mark"></p>
</div>
</html>


转载链接：http://www.cnblogs.com/Jimmy1988/p/7053875.html

