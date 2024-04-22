Latex是一个将语言和格式分开的标记语言
texdoc lshort-zh
```tex

\documentclass[UTF-8]{book}

\title{}
\author{}
\date{\today}

\begin{document}
\maketitle

\end{document}
```

## base

### 导言区

```tex
%导言区 引入文档类
\documentclass{book}
article 是用于论文的文档类
```

可以加一些参数
```tex
%字体大小为12pt,a4纸打印，单面
\documentclass[12pt, a4paper, oneside]{artical}
```

`{}` 表示命令的参数


### 宏包

```tex
%引入宏包
\usepackage{ctex}
```
### 正文区

```tex

\begin{document}
%增加空行表示回车

\end{document}

```


## 字体

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240412163626.png)

### 字体族设置

```tex
%罗马字体
\textrm{罗马字体}

%打字机字体
\texttt{打字机字体}


%设置字体族
{\rmfamily } 

\ttfamily

\sffamily


\songti
\heiti
\fangsong
\kaishu
```

### 字体形状
```tex
%加粗字体
\textbf{contents}

%斜体
\textit{contents}

%下划线
\underline
```

### 字体大小

根据normal size来等比例放缩

```tex
\tiny
\samll
\large
\Large
\LATGE
\huge
\Huge


%中文设置字号
\zihao{}
```

## 环境

### document
正文环境，一个LaTex文件有且仅有一个document环境

### equation
产生带编号的行间公式

### 伪代码

```tex
\begin{algorithm}
\caption{计算从1到n的和}              %标题
\label{alg1}                        %标记算法，方便在其它地方引用
\begin{algorithmic}
\REQUIRE$n \geq 1$                  %输入条件
\ENSURE$Sum = 1 + \cdots + n$      %输出
\STATE$Sum \leftarrow 0$            %\STATE 命名演示
\IF{$n < 1$}                        %条件语句
\PRINT{InputErrow}                %打印语句
\ELSE
    \FOR{$i = 0$ton}          %FOR循环结构
    \STATE$Sum = Sum + i$\\
    \STATE$i = i + 1$
    \ENDFOR
\ENDIF
\RETURNSum
\end{algorithmic}
\end{algorithm}

```


## newcommand

是为了更好的内容与格式分离，一键取消/新增格式，而不是在正文中出现大量命令

```tex
\newcommand{cmd}{def}
```

## 篇章结构

### 页面

设置页边距
```tex
\usepackage{geometry}
\geometry{left=2.54cm, right=2.54cm, top=3.18cm, bottom=3.18cm}
```
设置行间距
```tex
\linespread{1.5}
```

### 缩进


设置缩进一个em为一个m的宽度
```latex
\setlength{\parindent}{4em}
```
### 段间距

设置段间距为1em
```latex
\setlength{\parskip}{1em}
```
设置1.5倍行间距
```latex
\renewcommand{\baselinestretch}{1.5}
```

### 居中

```tex
\centerline{}

\begin{center}

\begin{center}
```

### section 小节


会自动排版排序
```tex
\section{计算机学科}

\subsection{四大件}

\subsubsection{数据结构}

\subsubsection{操作系统}

\subsubsection{计算机网络}

\subsubsection{数据库}

\subsection{综合素养}

\section{英语学科}

\subsection{四六级}
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240412165514.png)

`\par` 产生新段落

### chapter  章节


### tablecontents 文档目录

调整目录间距
```tex
\usepackage{setspace}


\begin{spacing}{2}
\tableofcontents
\end{spacing}

```

目录大小
```tex
{\small \tableofcontents}
```

目录字体大小
```tex
\renewcommand\cftchapfont{\heiti \zihao{4}} %chapter字号字体_

\renewcommand\cftsecfont{\heiti \zihao{-4}} % section字号字体_

\renewcommand\cftsubsecfont{\songti \zihao{-4}} % subsection字号字体_
```

### newpage换页


### hspace水平空间
```tex
\hspace{10cm}
```


## 特殊字符

使用空行分段，多个空行等于一个
默认自动缩进，不能用空格代替换行
多个空格等于一个

如果要产生空格
```tex
\quad
\qquad
\ 
%其余查阅手册
```

引号：
```tex
%` 左引号
%' 右引号
```

## 插图

使用`graphicx`宏包



```tex
%includegraphics[opts]{filename}
opts:
缩放因子
scale=0.3
图像高度
height=2cm
height=0.5\textheight
图像宽度
width=2cm
width=0.5\textwidth
旋转角度
angle=45




%格式 EPS ，PDF，JPG ，PNG，BMP
\graphicspath{{figures/}}%图片在当前目录的figures目录下
%也可添加其他路径，但要用{}实现分组
\graphcispath{{pt1},{pt2},{pt3}}



\begin{figure}

    \includegraphics[scale=0.5]{head.png}

    \caption{f**k u nvidia}

\end{figure}
```


## 表格

手册：
```text
booktable:有三线表绘制
longtab:跨页长表格
tabu:综合表格
```

```tex
使用tabular环境创建表格

c：居中对齐
l：左对齐
r:右对齐
p{1.5cm}:产生指定宽度的列

& 分割列
\\ 分割行

\begin{tabular}{c c c }

    id  & name & birth      \\

    001 & Tom  & 2003-08-03

\end{tabular}

产生表格竖线
{c|  c|c }



产生表格横线
\hline

\begin{tabular}{|c |c| c| }

    \hline

    id  & name & birth      \\

    \hline

    001 & Tom  & 2003-08-03 \\

    \hline

\end{tabular}
```

自动换行的表格
```tex
% for table
\usepackage{tabularx}


\begin{table*}[!htbp]
    \centering
    %设置行高
    \renewcommand\arraystretch{2}
    %\caption{A table with line breaks}
    \begin{tabularx}{\textwidth}{|X|}
          \hline
                %contents
          \hline
    \end{tabularx}%
    \label{tab:addlabel}%
   
\end{table*}%
```

## 浮动体

比如figure环境，table环境 
无序列表`itemize`、有序列表`enumerate`
使用`\centering`居中排版
使用`\caption`设置标题，表上图下，自动标号
使用`\label`设置标签
使用`\ref`引用标签
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20240412215746.png)

`[htbp]` 允许出现在各处位置


## 数学公式

如需排版，则需要有equation环境
```tex
\begin{eqution}
1+1=2
\end{equation}
```

### gather环境

使得有多行公式，并自动编号 +`\notag` 阻止编号

```tex
\begin{gather}
a+b=b+a\\
c*d=d*c
\end{gather}
```
$$
\begin{gather}
a+b=b+a\\
c*d=d*c
\end{gather}
$$

### align环境

指定对齐方式

按等号对齐：
```tex
\begin{align}
y&=kx+b\\
aaaa&=bbbbb
\end{align}
```
$$
\begin{align}
y&=kx+b\\
aaaa&=bbbbb
\end{align}
$$

按公式左端对齐：
```tex
\begin{align}
&y=kx+b\\
&aaaaa=bbbbb
\end{align}
```
$$
\begin{align}
&y=kx+b\\
&aaaaa=bbbbb
\end{align}
$$

### cases环境
分段函数


```tex
\begin{equation}
f(x)=
    \begin{cases}
    1,& \text{如果} x\in D\\
    0,&\text{如果} x\notin D 
    \end{cases}
\end{equation}
```
$$
\begin{equation}
f(x)=
    \begin{cases}
    1,& \text{如果} x\in D\\
    0,&\text{如果} x\notin D 
    \end{cases}
\end{equation}
$$

### matirx环境

与表格差不多，&分隔列，//分隔行
```tex
matrix环境
pmatrix 小括号
bmatrix 中括号
Bmatrix 大括号
vmatrix 竖线
Vmatirx 双竖线
```
```tex
\begin{Vmatrix}
1&1\\
2&2
\end{Vmatrix}
```
$$
\begin{Vmatrix}
1&1\\
2&2
\end{Vmatrix}
$$


## 参考文献

使用宏包


`\bibliographystyle{plain}`
编写文件bib文件,以键值对的形式展现，如：
```bib
@InProceedings{Vatashsky_2020_CVPR,  
author = {Vatashsky, Ben-Zion and Ullman, Shimon},  
title = {VQA With No Questions-Answers Training},  
booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},  
month = {June},  
year = {2020}  
}
```
  
在 BibTeX 条目中，作者字段通常使用 "and" 来分隔不同的作者，而不是逗号。这是 BibTeX 的约定和语法规则。当你使用逗号分隔作者时，BibTeX 可能会将整个列表视为一个作者的名字，导致格式错误。因此，为了确保 BibTeX 正确地解析作者列表，应该使用 "and" 分隔每个作者的名字。

```tex
\nocite{*} % 输出参考文献库中的所有条目


\bibliographystyle{plain}

\bibliography{works}
```

引用文献`\cite{Vatashsky_2020_CVPR}`
`\nocite{*}`排版所有未引用文献

## 中英文混排格式不齐问题

引入`sloppypar`环境，在该环境内进行正文编写

