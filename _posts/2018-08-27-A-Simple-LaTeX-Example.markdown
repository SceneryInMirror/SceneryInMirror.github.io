---
layout: post
title: "A simple LaTeX Example"
img: latex.jpg
date: 2018-08-27 22:02:00
category: blog
author: ykx
description: Learning how to use LaTeX.
tag: [Computer, LaTeX]
---

Reference: https://liam0205.me/2014/09/08/latex-introduction/

Tips1: http://www.360doc.com/content/14/0916/10/9561082_409854404.shtml

Tips2: https://liam0205.me/2013/10/17/LaTeX-Linespace/

Date: 2018.08.27

Author: ykx

~~~tex
%-------------------------------------------------------------------
% Reference: https://liam0205.me/2014/09/08/latex-introduction/
% Tips1: http://www.360doc.com/content/14/0916/10/9561082_409854404.shtml
% Tips2: https://liam0205.me/2013/10/17/LaTeX-Linespace/
% Date: 2018.08.27
% Author: ykx
%-------------------------------------------------------------------


%-------------------------------------------------------------------
% \控制序列（或称命令/标记）[可选参数]{参数}
% eg: \documentclass{article}
% Tips: 控制序列大小写敏感

% \documentclass[UTF8]{ctexart} % 以UTF-8编码保存，使用XeLaTeX编译，支持中文和中文版式
\documentclass[UTF8]{article} % 不支持中文
%-------------------------------------------------------------------


%-------------------------------------------------------------------
%%%%% 导言区 %%%%%
% \documentclass{article} 和 \begin{document}之间的内容被称为“导言区”
% 导言区中的控制序列，通常会影响到整个输出文档
% 通常在导言区中设置页面大小、页眉页脚样式、章节标题样式等等

% XeTeX原生支持Unicode，可以方便地调用系统字体，只需要使用几个宏包就能完成中文支持
% “宏包”就是一系列控制序列的合集，调用方式：\usepackage{·}
% CTeX宏集适用于多种编译方式，在内部处理好了中文和中文版式的支持（CTeX宏集和CTeX套装是两个不同的东西，前者是LaTeX宏的集合，包含文档类.cls文件和宏包.sty文件，后者是一个TeX系统）

\usepackage{xeCJK} % 使article支持中文
\setCJKmainfont{SimSun} % 设置CJK主字体为SimSun（宋体）；others: YouYuan(幼圆)
\usepackage{amsmath} % 使用AMS-LaTeX提供的数学功能
\usepackage{xfrac} % 提供\sfrac分式排版命令
\usepackage{graphicx} % 插入图片
\usepackage{booktabs} % 定义了三条划线命令：\toprule, \midrule, \bottomrule
\usepackage{geometry} % 设置页边距
\usepackage{fancyhdr} % 设置页眉页脚
\usepackage{setspace} % 调整行间距
\onehalfspacing
% \usepackage{indentfirst} % CTeX已经处理好首行缩进问题，如使用xeCJK宏包需要调用indentfirst宏包
% \setlength{\parindent}{\ccwd} % 调整首行缩进大小，\ccwd是当前字号下一个中文汉字的宽度

%-----------------------------
%%% 作者，标题，日期：
\title{Example}
\author{ykx}
\date{\today}

%-----------------------------
%%% 页边距：
\geometry{papersize={20cm,30cm}} % 纸张宽度20cm, 长度30cm
\geometry{left=1cm,right=2cm,top=3cm,bottom=4cm} % 左右上下边距：1cm, 2cm, 3cm, 4cm

%-----------------------------
%%% 页眉页脚：
\pagestyle{fancy}
\lhead{\author}
\chead{\date}
\rhead{152xxxxxxxx}
\lfoot{}
\cfoot{\thepage}
\rfoot{}
\renewcommand{\headrulewidth}{0.4pt}
\renewcommand{\headwidth}{\textwidth}
\renewcommand{\footrulewidth}{0pt}

%-----------------------------
%%% 修改段间距：
\addtolength{\parskip}{.4em} % 增加段间距0.4em
%-------------------------------------------------------------------


%-------------------------------------------------------------------
%%%%% 主体 %%%%%
% 控制序列begin和end成对出现，这两个控制序列及它们中间的内容称为“环境”
% 它们第一个参数总是一致的，称为“环境名”

\begin{document}

\maketitle % 插入作者，标题，日期
\tableofcontents % 插入目录，修改后要编译两次才会更新

你好，world! 这里是正文。

%-----------------------------
%%% 行文组织结构：
\section{这里是第一级标题}
\subsection{这里是第二级标题}
\subsubsection{这里是第三级标题}
\paragraph{这里是第一级段落}
\subparagraph{这里是第二级段落}
% 在report/ctexrep中还有\chapter{·}，在文档类book/ctexbook中还有\part{·}

%-----------------------------
%%% 数学公式：
$\text{行内公式使用\$...\$}: E = mc^2$
% 行内公式也可以使用\(...\)或者\begin{math}...\end{math}来插入
\[\text{行间公式使用} \setminus [... \setminus ]: G = mg\]
\begin{equation}
\text{公式环境使用equation}: x_{1,2} = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
\end{equation}
% 无编号行间公式也可以使用\begin{displaymath}...\end{displaymath}或者\begin{equation*}...\end{equation*}来插入
% $$...$$可以用来插入不编号的行间公式，但在LaTeX中这样会改变行文的默认行间距，不推荐

% 有关数学公式的其他说明：
% 公式标点使用的规范：行内公式的标点放在数学模式的限定符之外，行间公式的标点放在数学模式的限定符之内
% 强制分式输出效果：\dfrac强制行内模式的分式显示为行间模式的大小，\tfrac反之；行内分式可以尝试xfrac宏包提供的\sfrac命令；排版繁分式应该使用\cfrac命令
$\sfrac {-b \pm \sqrt{b^2 - 4ac}}{2a}$ \\
$\cfrac {-b \pm \sqrt{b^2 - 4ac}}{2a}$

%-----------------------------
%%% 运算符：
\[ \pm\; \times \; \div\; \cdot\; \cap\; \cup\;
\geq\; \leq\; \neq\; \approx \; \equiv\;
\sum\; \prod\; \lim\; \int\; \]
% 可以使用\limits和\nolimits来强制显式地指定是否压缩这些上下标
\[ \lim\limits_{n \rightarrow 0} \; \sum\limits_{n = 0}^N \]
\[ \lim\nolimits_{n \rightarrow 0} \; \sum\nolimits_{n = 0}^N \]
% 多重积分可以使用 \iint, \iiint, \iiiint, \idotsint 等命令输入
\[ \iint\quad \iiint\quad \iiiint\quad \idotsint \]
% \quad退一格，\qquad退两格

%-----------------------------
%%% 定界符（括号等）：
% 各种括号：
\[ (), [], \{\}, \langle\rangle, \lvert\rvert, \lVert\rVert \]

\[ \Biggl(\biggl(\Bigl(\bigl((x)\bigr)\Bigr)\biggr)\Biggr) \]
\[ \Biggl[\biggl[\Bigl[\bigl[[x]\bigr]\Bigr]\biggr]\Biggr] \]
\[ \Biggl \{\biggl \{\Bigl \{\bigl \{\{x\}\bigr \}\Bigr \}\biggr \}\Biggr\} \]
\[ \Biggl\langle\biggl\langle\Bigl\langle\bigl\langle\langle x
\rangle\bigr\rangle\Bigr\rangle\biggr\rangle\Biggr\rangle \]
\[ \Biggl\lvert\biggl\lvert\Bigl\lvert\bigl\lvert\lvert x
\rvert\bigr\rvert\Bigr\rvert\biggr\rvert\Biggr\rvert \]
\[ \Biggl\lVert\biggl\lVert\Bigl\lVert\bigl\lVert\lVert x
\rVert\bigr\rVert\Bigr\rVert\biggr\rVert\Biggr\rVert \]

%-----------------------------
%%% 省略号：
\[ x_1,x_2,\dots ,x_n\quad 1,2,\cdots ,n\quad
\vdots\quad \ddots \]

%-----------------------------
%%% 矩阵：
\[ \begin{pmatrix} a&b\\c&d \end{pmatrix} \quad
\begin{bmatrix} a&b\\c&d \end{bmatrix} \quad
\begin{Bmatrix} a&b\\c&d \end{Bmatrix} \quad
\begin{vmatrix} a&b\\c&d \end{vmatrix} \quad
\begin{Vmatrix} a&b\\c&d \end{Vmatrix} \]

% 使用 smallmatrix 环境，可以生成行内公式的小矩阵
Marry has a little matrix $ ( \begin{smallmatrix} a&b\\c&d \end{smallmatrix} ) $.

%-----------------------------
%%% 多行公式：
% 无须对齐的长公式可以使用 multline 环境
\begin{multline}
x = a+b+c+{} \\
d+e+f+g
\end{multline}
% 需要对齐的公式，可以使用 aligned 次环境来实现，它必须包含在数学环境之内
\[\begin{aligned}
x ={}& a+b+c+{} \\
&d+e+f+g
\end{aligned}\]
% 无需对齐的公式组可以使用 gather 环境，需要对齐的公式组可以使用 align 环境
\begin{gather}
a = b+c+d \\
x = y+z
\end{gather}
\begin{align}
a &= b+c+d \\
x &= y+z
\end{align}
% 分段函数可以用cases次环境来实现，它必须包含在数学环境之内
\[ y= \begin{cases}
-x,\quad x\leq 0 \\
x,\quad x>0
\end{cases} \]

%-----------------------------
%%% 插入图片和表格：
% 插入图片使用'\includegraphics': 
\includegraphics[width = .8\textwidth]{a.jpg} \\
% 插入表格：
\begin{tabular}{|l|c|r|}
 \toprule
操作系统& 发行版& 编辑器\\
 \hline
Windows & MikTeX & TexMakerX \\
 \midrule
Unix/Linux & teTeX & Kile \\
 \hline
Mac OS & MacTeX & TeXShop \\
 \hline
通用& TeX Live & TeXworks \\
 \bottomrule
\end{tabular}
% 浮动体：插图和表格通常需要占据大块空间，所以在文字处理软件中我们经常需要调整他们的位置。figure 和 table 环境可以自动完成这样的任务；这种自动调整位置的环境称作浮动体(float)
\begin{figure}[htbp]
\centering
\includegraphics[width = .8\columnwidth]{a.jpg}
\caption{有图有真相}
\label{fig:myphoto}
\end{figure}
% “htbp” 选项用来指定插图的理想位置，这几个字母分别代表here, top, bottom, float page，也就是就这里、页顶、页尾、浮动页(专门放浮动体的单独页面) 。\centering 用来使插图居中；\caption 命令设置插图标题，LaTeX 会自动给浮动体的标题加上编号。注意 \label 应该放在标题命令之后


\end{document}
% \end{document}之后的内容无效
%-------------------------------------------------------------------
~~~
