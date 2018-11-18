
<html>
<div id="cnblogs_post_body" class="blogpost-body cnblogs-markdown"><h2 id="概述">概述</h2>
<p>最近在搞Markdown的东西，遇到了一个很棘手的问题，即图片的显示：通用的图片，可以直接网上搜索，但有时候需要自己截一些图或者对下载的图片进行修改，在本地存储完全没有问题，但Markdown写出来的文本并不是给自己看的，本地的MD文件传给别人时，图片无法显示。<br>
为了解决以上问题，搜索了一下，知道了“图床”这个名词，且很多人推荐使用<strong>七牛云存储</strong>作为图床(高效、快速、有保障)。但七牛云<strong>上传图片-&gt;复制图片地址</strong>一系列的流程比较麻烦，为了节省时间，逐选择了<strong>Mpic</strong> 图床神器，两者结合，真是无往不利。</p>
<hr>
<h2 id="mpic如何设置账号">Mpic如何设置账号</h2>
<h3 id="打开mpic点击设置账号">1. 打开Mpic，点击“设置账号”</h3>
<p><img src="https://mpic.kf5.com/attachments/download/1171222/0015808404e34f1e31116ea6cd92ed2/" alt="设置账号"></p>
<h3 id="填写七牛云的账号如果没有请自行申请">2. 填写<strong>七牛云</strong>的账号，如果没有，请自行申请</h3>
<blockquote>
<p><strong>七牛云存储地址</strong><br>
　　<a href="https://www.qiniu.com/" class="uri">https://www.qiniu.com/</a></p>
</blockquote>
<h3 id="注册完成之后将七牛对应信息填入面板">3. 注册完成之后，将七牛对应信息填入面板</h3>
<p><img src="https://mpic.kf5.com/attachments/download/1171231/001580840995e265db04376c8e74582/" alt="Mpic页面"></p>
<blockquote>
<p><strong>空间名称：</strong> 七牛存储文件的空间名（就是你用于存储文件的文件夹名字，如下图我的）<br>
<strong>AccessKey：</strong> 点击七牛“个人面板”，“密钥管理”中即可看到AccessKey<br>
<strong>SecretKey：</strong> 点击七牛“个人面板”，“密钥管理”中即可看到 SecretKey<br>
<strong>域名：</strong> 存储空间对应的域名，如果存储空间绑定了自己的域名就填写自己的域名</p>
</blockquote>
<p><strong>空间名称：</strong><br>
<img src="http://ory0rmuh5.bkt.clouddn.com/blog/171208/lDbcmbie1L.png" alt="space"></p>
<p><strong>AK&amp;SK:</strong><br>
<img src="http://ory0rmuh5.bkt.clouddn.com/blog/170624/LFf775G4el.jpg" alt="AK&amp;SK"></p>
<p><strong>域名</strong>：<br>
<img src="http://ory0rmuh5.bkt.clouddn.com/blog/171208/b4mLdlkjIk.png" alt="Domain"></p>
<p>当然，域名也可以在下图位置选择：</p>
<p><img src="http://ory0rmuh5.bkt.clouddn.com/blog/171208/0BeHhbb7a5.png" alt="mark"></p>
<h3 id="至此七牛云账号登录完毕将图片随意拖动到mpic窗口即完成了图片的上传">4. 至此，七牛云账号登录完毕，将图片随意拖动到Mpic窗口，即完成了图片的上传。</h3>
<hr>
<h2 id="截图工具----fscapture">截图工具 -- FScapture</h2>
<p>一个良好的截图和修图工具，使得Markdown的书写事半功倍，截图工具有很多，比如Windows7(及以上版本)自带的Snip截图(运行中输入：snippingtool)、PicPick等，但此处我推荐使用<strong>FScapture</strong>，其原因如下：</p>
<blockquote>
<ul>
<li>运行时内存低，不到1M<br>
</li>
<li>可截取视频</li>
<li>屏幕标尺，屏幕取色，屏幕放大镜，<strong>屏幕录像机</strong>（可添加片头说明）</li>
<li>捕捉活动窗口，捕捉窗口/对象，捕捉矩形区域，捕捉手绘区域，捕捉整个屏幕，捕捉滚动窗口</li>
<li>内置图片编辑器<br>
<img src="http://ory0rmuh5.bkt.clouddn.com/blog/170624/gK68ccIKBg.png?imageslim" alt="mark"></li>
</ul>
</blockquote>
<h3 id="主要功能介绍">主要功能介绍</h3>
<h4 id="截屏">1. 截屏</h4>
<p>包括了全屏截取，当前活动窗口截取，截取选定区域，多边形截取和截取滚动页面等，基本上常用的都有了。特别是滚动截取，许多朋友为了这个功能，不惜安装各种重量级的截屏软件，甚至四处下载各种软件的破解补丁。 <img src="https://images2015.cnblogs.com/blog/738404/201702/738404-20170220223847585-1850524366.png"></p>
<h4 id="图像浏览-编辑">2. 图像浏览 / 编辑</h4>
<p>FS Capture还包括快速（浏览/编辑图像）的功能，可以点击主窗口的“打开”图标快速打开一幅图片，进行简单的缩放、裁切、旋转、加文字等轻量级的操作。把网页中图片拖到FS Capture 的窗口上，会快速打开图像浏览窗口。<br>
<img src="http://ory0rmuh5.bkt.clouddn.com/blog/170624/FmB1CDK0F2.png?imageslim" alt="mark"></p>
<h4 id="视频录制">3. 视频录制</h4>
<p>7.0 版本开始具备的功能，只需点击“视频录制”按钮，即可选择一个录制范围，可以选择“Window/Object”（窗口或对象）、“Rectangular Area”(矩形区域)、“Full Screen Without Taskbar”(无任务栏全屏)、“Full Screen”(全屏)等范围。选择范围后，即可点击 Record 按钮，非全屏范围，还需要选择好一个区域，然后在弹出的窗口点，击“Start”按钮，即可开始录制了，最后可以按F11键停止。<br>
录制的过程如“视频录制”视频所示。录制的视频格式是 Wmv，录制完成后，会打开媒体播放器，进行播放。7.3 版本开始，支持同时录制麦克风和扬声器的音频。<br>
<img src="https://images2015.cnblogs.com/blog/738404/201702/738404-20170220224218445-2017717268.png"></p>
<h4 id="小功能介绍">4. 小功能介绍</h4>
<p>附带的其他两个小功能：取色器和屏幕放大镜。对抓取的图像，提供缩放、旋转、剪切、颜色调整等功能。只要点点鼠标，就能随心抓取屏幕上的任何东西，拖放支持可以直接从系统、浏览器或其他程序中导入图片。<br>
<img src="https://images2015.cnblogs.com/blog/738404/201702/738404-20170220224258304-2146205876.png"></p>
<h4 id="屏幕取色器">5. 屏幕取色器</h4>
<p>现在网上各式各样的取色器应该不少了，包括之前一直用的蓝色经典推荐的 ColorSPY， Firefox 下还有一个，专门的取色器扩展 ColorZilla，这些都是很好的软件。但自从使用了 FS Capture 之后，这些我都很少用到了。原因很简单，各种取色软件的功能，都大同小异，FS Capture 非常小巧，既然有这样一个小软件，能够包含取色器、屏幕放大镜和截屏的功能，为什么还要为这些功能，而分开多个软件呢？FastStone Capture 的取色支持 RGB、Dec 和 Hex 三种格式的色值，而且还有一个混色器，取到颜色之后，可以再编辑。</p>
<h4 id="屏幕放大镜">6. 屏幕放大镜</h4>
<p>这确实是一个不错的功能，特别是现在，我们已经习惯用 DIV 来对页面定位，DIV之间的对齐不像表格那样容易控制，有时为了调整几个像素的偏差，不得不对着屏幕盯很久。有这样一个放大镜，就方便多了。使用时，只需点击一下，FS Capture 窗口上的放大镜图标，鼠标变成一个放大镜的样子，然后在需要放大的地方，按下右键就可以了，就像手里真的拿着一个放大镜一样。可以设置放大倍率，放大镜的尺寸，外观（圆形，矩形以及圆角矩形）以及是否平滑显示，按 ESC 键或单击右键可退出放大镜。</p>
<h4 id="屏幕标尺">7. 屏幕标尺</h4>
<p>FastStone Capture 还有屏幕标尺功能， 点击后会屏幕上会出现一个尺子，方便测试屏幕某区域的像素大小。</p>
<h4 id="将图像转换为-pdf-文件">8. 将图像转换为 PDF 文件</h4>
<p>图片可以直接转为PDF</p>
<h4 id="发送到-powerpointwordftp">9.发送到 PowerPoint，Word，FTP</h4>
<p>截图后可一键发送到文档中正在编辑的位置，非常方便</p>
<hr>
<p><strong>七牛云</strong>+ <strong>Mpic</strong> + <strong>FScapture</strong> 强大到爆表，简直没谁了！</p>
<p><img src="http://ory0rmuh5.bkt.clouddn.com/blog/170624/eKHFmk26L6.png?imageslim" alt="mark"></p>
</div>
</html>
转载链接：https://www.cnblogs.com/Jimmy1988/p/7074423.html
