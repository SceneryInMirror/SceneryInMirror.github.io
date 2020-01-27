---
layout: post
title: "Vim Configuration"
img: vim.jpg
date: 2018-04-02 10:26:00
category: blog
author: ykx
description: Follow the guidance on the Internet to configure my Vim with the aim of making it convenient.
tag: [Vim, Computer, Learning]
---

### 参考文献

* Vim的简单配置：[https://blog.csdn.net/lhy2932226314/article/details/69668891](https://blog.csdn.net/lhy2932226314/article/details/69668891)

* 消除搜索后的关键字高亮：[https://blog.csdn.net/shaoshaoh/article/details/1694451](https://blog.csdn.net/shaoshaoh/article/details/1694451)

* su密码修改：[https://blog.csdn.net/david_xtd/article/details/7229325](https://blog.csdn.net/david_xtd/article/details/7229325)

* Vim跨文件复制粘贴：[https://www.cnblogs.com/yoyo-sincerely/p/5866206.html](https://www.cnblogs.com/yoyo-sincerely/p/5866206.html)

* 同一窗口打开多个终端并使用快捷键切换：[https://blog.csdn.net/u013623867/article/details/19022923](https://blog.csdn.net/u013623867/article/details/19022923)

### 配置文件.vimrc
~~~C
"======================
"File Settings
"======================

filetype on                 " 检测文件类型
"filetype indent on         " 不同文件不同缩进方式
"filetype plugin on         " 允许插件
filetype plugin indent on   " 启动自动补全
"set filetype=c

"syntax enable              " 开启语法高亮
syntax on                   " 开启语法高亮

"set autoread               " 文件修改后自动读入
"set guifont=Monaco\ 12     " 设置字体
set history=2000            " 设置历史记录条数
"set mouse=a                " Vim中可以使用鼠标
set nobackup                " 设置取消备份
set noswapfile              " 禁止临时文件生成


"======================
"Display Settings
"======================

set laststatus=2            " 总是显示状态栏
set lbr                     " 不在单词中间折行
set nowrap                  " 指定不折行
set nu                      " 显示行号(nu)
set ruler                   " 显示当前行号列号
set scrolloff=1             " 光标移动至少保留的行数
set showcmd                 " 在状态栏显示正在输入的命令
set showmatch               " 设置代码匹配，包括括号匹配情况
set showmode                " 左下角显示当前Vim模式

"highlight Function cterm=bold, ctermbg=red


"======================
"Format Settings
"======================

" 设置代码折叠
"set foldenable
"set foldmethod=indent
"set foldlevel=99
"manual                     " 手工折叠
"indent                     " 缩进折叠
"expr                       " 表达式折叠
"syntax                     " 语法折叠
"diff                       " 对没有更改的文件折叠
"marker                     " 标记折叠

" 设置C/C++方式自动对齐
set autoindent
set cindent
set smartindent

set shiftwidth=4            " 设置自动对齐空格数
set softtabstop=4           " 按退格键时可以一次删除4个空格

set et                      " 编辑时可以将所有tab设置为空格(expandtab)
set smarttab                " 使用backspace直接删除tab
set tabstop=4               " 设置tab宽度


"======================
"Search Settings
"======================

set hls                     " 设置搜索高亮(hlsearch)，用noh取消高亮
"set ignorecase             " 设置搜索时忽略大小写
set incsearch               " 开启及时搜索(is)
"set smartcase              " 搜索时尝试smart，智能大小写匹配


"======================
"FileEncode Settings
"======================

set encoding=utf-8          " 设置编码方式
set ffs=unix,dos,mac        " 将UNIX作为标准文件类型
" 设置打开文件的编码格式
"set fileencoding=ucs-bom,utf-8,cp936,gb18030,big5,euc-jp,euc-kr,latinl 
set formatoptions+=m        " 如遇Unicode值大于255的文本，不必等到空格再折行
set formatoptions+=B        " 合并两行中文时，不在中间加空格
set helplang=cn             " 显示中文
set termencoding=utf-8      " 只对终端影响（默认）


"======================
"Theme Settings
"======================

set background=dark
"colorscheme molokai
"colorscheme solarized
set t_Co=256

"set guioptions+=b          " 添加水平滚动条
"set guioptions-=m           " 取消菜单栏
"set guioptions-=T           " 取消导航栏
~~~

### 消除搜索高亮

* :noh

### su修改密码

* 设置root密码：sudo passwd root

* su和sudo的区别：su的密码是root的密码，sudo的密码是用户的密码；su直接将身份变成root，sudo是以用户身份登录以后以root的身份运行命令，不需要知道root密码

### Vim跨文本复制粘贴

* 在普通模式下输入:sp横向切分一个窗口，或者:vsp纵向切分一个窗口

* 在普通模式下输入:e [filename]，在其中一个窗口打开另一个文件

* 两个窗口切换：普通模式下ctrl+w，再按一下w就可以切换

### 同一窗口打开多个Terminal

* ctrl+shift+t：打开另一个tab

* alt+n：切换tab

* ctrl+page_down/page_up：切换tab

* ctrl+shift+w：关闭当前tab
