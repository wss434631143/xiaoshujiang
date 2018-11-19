---
title: Shell脚本开发环境的配置和优化实践 
weblog_mt_keywords: "vim,shell"
---
## 一.配置vim编辑器

### 1.为什么不使用vi而是vim
 - vi适合编辑普通文本，不适用编写脚本代码，例如：缺少高亮显示代码、自动缩进等重要功能；
 - vim相当于高级编辑器，可以提高开发效率。

----------


### 2.设置vim为默认编辑器

```shell
[root@oldboy scripts]# echo 'alias vi=vim' >> /etc/bashrc
[root@oldboy scripts]# tail -1 /etc/bashrc
alias vi=vim
[root@oldboy scripts]# source /etc/bashrc
```
经过上述调整后，当用vi命令时，会自动被vim替换。

----------


### 3.配置.vimrc的重要参数
Linux环境下的vim编辑器默认功能不够强大，如果要进行Shell脚本的开发，还需要进行适当的设置，从而达到高效开发的目的。vim编辑器有一个可以用来调整配置的配置文件，默认放置在用户家目录下，全路径及名字组合为：~/.vimrc（全局路径为/etc/vimrc），这是一个隐藏文件。

~/.vimrc配置内容如下：

```vim
cat > ~/.vimrc << eof
" ~/.vimrc
" vim config file
" date 2018-07-15
" Created by oldboy
" blog:http://www.cnblogs.com/wushuaishuai
"""""""""""""""""""""
" => 全局配置
"""""""""""""""""""""
"关闭兼容模式
set nocompatible
"设置历史记录步数
set history=100
"开启相关插件
filetype on
filetype plugin on
filetype indent on
"当文件在外部被修改时，自动更新该文件
set autoread
"激活鼠标的使用
set mouse=a
"""""""""""""""""""""
" => 字体和颜色
"""""""""""""""""""""
"开启语法
syntax enable
"设置字体
"set guifont=dejaVu\ Sans\ MONO\ 10
"
""设置配色
"colorscheme desert
"高亮显示当前行
set cursorline
hi cursorline guibg=#00ff00
hi CursorColumn guibg=#00ff00
"""""""""""""""""""""
" => 代码折叠功能 by oldboy
"""""""""""""""""""""
"激活折叠功能
set foldenable
"设置按照语法方式折叠（可简写set fdm=XX）
"有6种折叠方法：
"manual 手工定义折叠
"indent 更多的缩进表示更高级别的折叠
"expr   用表达式来定义折叠
"syntax 用语法高亮来定义折叠
"diff   对没有更改的文本进行折叠
"marker 对文中的标志进行折叠
set foldmethod=manual
"设置折叠区域的宽度
"如果不为0，则在屏幕左侧显示一个折叠标识列
"分别用“-”和“+”来表示打开和关闭的折叠。
set foldcolumn=0
"设置折叠层数为3
setlocal foldlevel=3
"设置为自动关闭折叠
set foldclose=all
"用空格键来代替zo和zc快捷键实现开关折叠
"zo  O-pen a fold   (打开折叠)
"zc  C-lose a fold  (关闭折叠)
"zf  F-old creation (创建折叠)
nnoremap <space> @=((foldclosed(line('.')) < 0)  'zc' : 'zo')<CR>
"""""""""""""""""""""
" => 文字处理 by oldboy
"""""""""""""""""""""
"使用空格来替换Tab
set expandtab
"设置所有的Tab和缩进为4个空格
set tabstop=4
"设定 << 和 >> 命令移动时的宽度为4
set shiftwidth=4
"使得按退格键时可以一次删掉4个空格
set softtabstop=4
set smarttab
"缩进，自动缩进(继承前一行的缩进)
"set autoindent命令关闭自动缩进，是下面配置的缩写。
"可使用autoindent命令的简写，即 “:set ai” 和 “:set noai”。
"还可以使用“ :set ai sw=4”在一个命令中打开缩进并设置缩进级别。
set ai
"智能缩进
set si
"自动换行
set wrap
"设置软宽度
set sw=4
"""""""""""""""""""""
" => Vim 界面 by oldboy
"""""""""""""""""""""
"Turn on WiLd menu
set wildmenu
"显示标尺
set ruler
"设置命令行的高度
set cmdheight=1
"显示行数
"set nu
"Do not redraw, when running macros.. lazyredraw
set lz
"设置退格
set backspace=eol,start,indent
"Bbackspace and cursor keys wrap to
set whichwrap+=<,>,h,l
"Set magic on（设置魔术）
set magic
"关闭遇到错误时的声音提示
"关闭错误信息响铃
set noerrorbells
"关闭使用可视响铃代替呼叫
set novisualbell
"显示匹配的括号([{和}])
set showmatch
"How many tenths of a second to blink
set mat=2
"搜索时高亮显示搜索到的内容
set hlsearch
"搜索时不区分大小写
"还可以使用简写（“:set ic” 和 “:set noic”）
set ignorecase
"""""""""""""""""""""
" => 编码设置
"""""""""""""""""""""
"设置编码
set encoding=utf-8
"设置文件编码
set fileencodings=utf-8
"设置终端编码
set termencoding=utf-8
"""""""""""""""""""""
" => 其他设置 by oldboy 2010
"""""""""""""""""""""
"开启新行时使用智能自动缩进
set smartindent
set cin
set showmatch
"隐藏工具栏
set guioptions-=T
"隐藏菜单栏
set guioptions-=m
"置空错误铃声的终端代码
set vb t_vb=
"显示状态栏 (默认值为 1, 表示无法显示状态栏)
set laststatus=2
"粘贴不换行问题的解决方法
set pastetoggle=<F9>
"设置背景色
set background=dark
"设置高亮相关
highlight Search ctermbg=black  ctermfg=white guifg=white guibg=black
"在Shell脚本的开头自动增加解释器及作者等版权信息
autocmd BufNewFile *.py,*.cc,*.sh,*.java exec ":call SetTitle()"
func SetTitle()
   if expand("%:e") == 'sh'
    call setline(1, "#!/bin/bash")
    call setline(2, "# Author: Wu ShuaiShuai")
    call setline(3, "# Blog2: http://www.cnblogs.com/wushuaishuai")
    call setline(4, "# Time: ".strftime("%F %T"))
    call setline(5, "# Name: ".expand("%"))
    call setline(6, "# Version: v1.0")
    call setline(7, "# Description: This is a Script.")
   endif
endfunc
eof
```
退出重新登录后.vimrc生效,同样适用于普通用户。

----------


### 4.vim路径等配置知识

| 相关配置文件                 | 功能描述              |
| ---------------------------- | --------------------- |
| .viminfo                     | 用户使用vim的操作历史 |
| .vimrc                       | 当前用户vim的配置文件 |
| /etc/vimrc                   | 系统全局vim的配置文件 |
| /usr/share/vim/vim74/colors/ | 配色模板文件存放路径  |



----------


## 二.vim编辑器实战

### 1.代码自动缩进功能
当输入循环及条件结构语句等代码时，系统会自动将输入语句的关键字及命令代码缩进到合理的位置，可以看到，vim的配置是以两个空格为缩进宽度（.vimrc里设置的）的。

----------


### 2.代码颜色高亮显示功能
代码颜色高亮显示也是一个非常好的功能，可以通过它区分字符、变量、循环等很多不同的Shell脚本元素。例如当编写的代码出现错误时，对应的代码高亮颜色就会和正确时的不同，开发者可以根据代码的高亮颜色对Shell脚本进行调试，提升编码的效率，减少编码的错误。

----------


### 3.vim配置文件的自动增加版权功能
当执行“vim oldboy.sh”编辑脚本时，只要是以.sh为扩展名的，就会自动增加版权信息。

----------

### 4.显示当前行、显示光标的坐标位置
![显示当前行、显示光标的坐标位置](http://pb4ob7u50.bkt.clouddn.com/xiaoshujiang/2018715/1531636568835.png)

----------

### 5.vim配置文件的代码折叠功能
在代码量较大时比较有用的高级功能——代码折叠（依赖.vimrc配置，当然也可以以命令模式执行）    

- 在命令模式下，可以把光标定位到当前的第2行，然后执行*zf3j*命令，便可将第2行及其下的3行缩进，其他缩进也是如此。
- 若把光标放到对应折叠后的行上，*按空格键*即可展开上述折叠的行。

### 6.vim编辑器批量缩进及缩进调整技巧
有时我们从外部复制部分Shell代码到当前脚本后发现缩进是乱的。

- 此时可以将vim编辑器调整为命令模式（*按Esc键*），==然后移动键盘上下键==将光标定位到要调整的行开头。

- 接下来*输入“v”（可视化缩写）*，然后*用键盘移动光标选定要调整的多行*。

- 最后*按“=”键*即可将代码调整为规整的格式。

----------

### 7.vim编辑器块操作

#### 可视模式
**进入可视模式有三种方法：v,V,Ctrl+v**
- 按v(小写)启用可视模式，可以按单个字符选择内容，移动光标可以选择。
- 按V(大写)启用可视模式，立刻选中光标所在行，按单行符选择内容，移动光标可以选择。
- 按Ctrl+v启用可视中的列块模式，可以在列方向上选择单个字符，移动光标可以选择。

#### 列块(可视)模式
**Ctrl+v,启用列块模式，之后移动鼠标，可以选中某一个矩形块，对于有规律的表格可以用这个功能。**
**目前当前光标所在的位置是右下角，可以在这个块的四角进行移动光标，方法就是按o(小写)，O(大写)来切换四个顶点。**

**(1).删除或剪切操作** 
- Ctrl+v,进入列块模式，选中需要删除的内容
- 按d(小写)即可删除选中区域
- 按D(大写)删除选中区域及所在行后面的数据

**(2).输入操作** 
- Ctrl+v,进入列块模式，选中需要添加的内容
- 按I(大写)进入插入，然后输入“//”（你想输入的字符）
- 然后按ESC即可在其它行输入你想输入的字符了

**(3).复制和粘贴操作** 
- Ctrl+v,进入列块模式，选中需要复制的内容
- 按y(小写)复制内容，4 line yanked 说明复制了四行
- 然后移动光标到行首，按p(小写)在光标的后面一列输入内容，按P(大写)在光标前面一列输入内容

----------

### 8.vim多窗口使用技巧

#### 打开多窗口
打开多窗口的命令以下几个：

**横向切割窗口**

``` vim
:new+窗口名(保存后就是文件名) 
:split+窗口名，也可以简写为:sp+窗口名
```

**纵向切割窗口名**

``` vim
:vsplit+窗口名，也可以简写为：vsp+窗口名
```

#### 关闭多窗口

``` vim
可以用：q!，也可以使用：close，最后一个窗口不能使用close关闭。使用close只是暂时关闭窗口，其内容还在缓存中，只有使用q!、w!或x才能真能退出。
:tabc 关闭当前窗口
:tabo 关闭所有窗口
```

#### 窗口切换

``` vim
:ctrl+w+j/k，通过j/k可以上下切换，或者:ctrl+w加上下左右键，还可以通过快速双击ctrl+w依次切换窗口。
```

#### 窗口大小调整

**纵向调整**

``` vim
:ctrl+w + 纵向扩大（行数增加）
:ctrl+w - 纵向缩小 （行数减少）
:res(ize) num  例如：:res 5，显示行数调整为5行
:res(ize)+num 把当前窗口高度增加num行
:res(ize)-num 把当前窗口高度减少num行
```

**横向调整**

``` vim
:vertical res(ize) num 指定当前窗口为num列
:vertical res(ize)+num 把当前窗口增加num列
:vertical res(ize)-num 把当前窗口减少num列
```

#### 给窗口重命名

``` vim
:f file
```

#### vim打开多文件

``` vim
vim a b c
:n 跳至下一个文件，也可以直接指定要跳的文件，如:n c，可以直接跳到c文件
:e# 回到刚才编辑的文件
```

#### 开启目录浏览器

``` vim
:Ex 开启目录浏览器（可以浏览当前目录下的所有文件，并且可以选择）
:Vex 垂直分割窗当前窗口，并在一个窗口中开启目录浏览器
:Sex 水平分割当前窗口，并在一个窗口中开启目录浏览器
:ls 显示当前buffer情况
```

#### vim与shell切换

``` vim
:shell 可以在不关闭vi的情况下切换到shell命令行
exit 从shell回到vim
```  




----------


### 9.vim常用操作快捷键

| 命令 | 说明 |
| :-- | --- |
| **普通模式：移动光标的操作** ||
| G 或(shift+g) | 将光标移动到文件的最后一行 |
| gg | 将光标移动到文件的第一行，等价于1gg 或1G |
| 数字0或^ | 将光标从所在位置移动到当前行的开头 |
| $ | 从光标所在位置将光标移动到当前行的结尾 |
| n+Enter键 | n 为数字，Enter键 为回车键，将光标从当前位置向下移动 n 行 |
| ngg | n 为数字，移动到文件的第n 行，如11gg 可移动到第 11 行，可配合“:set nu ”查看，同nG |
| H | 光标移动到当前窗口最上方的那一行 |
| M | 光标移动到当前窗口中间的那一行 |
| L | 光标移动到当前窗口最下方的那一行 |
| h 或( ←) | 光标向左移动一个字符 |
| j 或（↓） | 光标向下移动一个字符 |
| k 或（↑） | 光标向上移动一个字符 |
| l 或（→） | 光标向右移动一个字符 |
| **普通模式：搜索与替换操作** ||
| /oldboy | 从光标位置开始，向下寻找名为oldboy 的字符串 |
| ?oldboy | 从光标位置开始，向上寻找名为oldboy 的字符串 |
| n | 从光标位置开始，向下重复前一个搜索的的动作 |
| N | 从光标位置开始，向上重复前一个搜索的的动作 |
| :g/A/s//B/g | 把符合A 的内容全部替换为B ，斜线为分隔符，可以用@ 、# 等替代 |
| :%s/A/B/g | 把符合A 的内容全部替换为B ，斜线为分隔符，可以用@ 、# 等替代 |
| :n1,n2s/A/B/gc | n1 、n2 为数字，在第n1 行和n2 行间寻找A ，用B 替换 |
| **普通模式：复制、粘贴、删除等操作** ||
| yy | 复制光标所在的当前行 |
| nyy | n 为数字，复制光标开始向下共n 行 |
| p/P | p 将已复制的数据粘贴到光标的下一行，P 则为粘贴到光标的上一行 |
| dd | 删除光标所在的当前行 |
| ndd | n 为数字，删除从光标开始向下共n 行 |
| u | 恢复（回滚）前一个执行过操作 |
| . | 点号。重复前一个执行过的动作 |
| x | 向后删除字符 |
| X | 向前删除字符 |
| d1G | 删除当前行至第一行 |
| dG | 删除当前行至最后一行 |
| d0 | 删除当前光标文本至行首 |
| d$ | 删除当前光标文本至行尾 |
| **进入编辑模式命令** ||
| i | 在当前光标所在处插入文字 |
| a | 在当前光标所在下一个字符处插入文字 |
| I | 在当前所在行的行首第一个非空格符处开始插入文字，和 A 相反 |
| A | 在当前所在行的行尾最后一个字符处开始插入文字，和 I 相反 |
| O | 在当前所在行的上一行处插入新的一行 |
| o | 在当前所在行的下一行处插入新的一行 |
| Esc | 退出编辑模式，回到命令模式中 |
| **命令行模式** ||
| :wq | 退出并保存 |
| :wq! | 退出并强制保存，"!" 为强制的意思 |
| :q! | 强制退出，不保存 |
| :n1,n2 w filename | n1 、n2 为数字，将 n1 行到 n2 行的内容保存成 filename 这个文件 |
| :n1,n2 co n3 | n1 、n2 为数字，将 n1 行到 n2 行的内容拷贝到 n3 位置下 |
| :n1,n2 m n3 | n1 、n2 为数字，将 n1 行到 n2 行的内容挪至 n3 位置下 |
| :!command | 暂时离开vi 到命令行模式下执行command 的显示结果！例如 :! ls /etc |
| :set nu | 显示行号 |
| :set nonu | 与 set nu 相反，取消行号 |
| :vs filename | 垂直分屏显示，同时显示当前文件和 filename 对应文件的内容 |
| :sp filename | 水平分屏显示，同时显示当前文件和 filename 对应文件的内容 |
| I + # + Esc | 在可视块模式下（Ctrl + v ）, 一次性注释所选的多行，取消注释可用:n1,n2s/#/ /gc |
| Del | 在可视块模式下（Ctrl + v ），一次性删除所选内容 |
| r | 在可视块模式下（Ctrl + v ），一次性替换所选内容 |  
![Vim/Vi 工作模式](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181119/1542595580548.png)

![Vim 命令](https://www.github.com/wss434631143/xiaoshujiang/raw/master/img/20181119/1542595603404.png)

----------

        
        
> 参考资料：《跟老男孩学Linux运维：Shell编程实战》