---
layout:     post   				    
title:      如何在ubuntu18以及vim下配置texlive以及中文环境
subtitle:   tutorial
date:       2020-04-10
author:     James Pan
header-img: img/post-bg-2015.jpg 	
catalog: true 						
tags:								
    - ubuntu
    - texlive
    - vim
---
# 前言
最近想要学习latex以便做简历。然后自己用的操作系统是ubuntu18.04，习惯用vim作为各种软件和高级语言的文本编辑器，所以也想着用vim来完成对latex的编译，做一个忠实的vim粉。经过艰难的探索后决定写这篇文章来记录一下安装latex的过程，方便以后参考。转载请注明出处，感谢。

# 平台和配置
+ ubuntu18.04
+ vim8.2
+ vim相关插件：vimtex

# 安装过程
安装Texlive和中文包
```bash
sudo apt install texlive
sudo apt install texlive-lang-chinese
```
安装完之后想要创建.tex文件来进行简单的测试，发现报错
```vim
vimtex: compiler not initialized
```
此后通过搜索发现，vimtex需要latexmk来协助编译。于是安装latexmk
```bash
sudo apt install latexmk
```
安装完成后创建一个简单的后缀为.tex的文件进行测试，内容如下
```latex
\documentclass{article}

\begin{document}

hello,world

\end{document}
```
结果是编译成功，然后尝试输入中文进行测试，内容如下
```latex
\documentclass{article}

\usepackage{xeCJK}

\begin{document}

hello,你好

\end{document}
```
然后在vim的界面里面报错
```vim
File `xeCJK.sty' not found.
```
然后发现脑残的自己忘记装支持中文编译的引擎，于是要继续安装引擎
```bash
sudo apt install texlive-xetex
```
这里安装的其实就是XeLaTex引擎，和其他引擎不同，目前是这个编译引擎对中文的支持效果最好，具体可以参考最下方的文章。之后继续测试，发现报错
```vim
/usr/share/texlive/texmf-dist/tex/xelatex/xecjk/xeCJK.sty|43 error| Critical xeCJK error: "Require-XeTeX"
You must change your typesetting engine to "xelatex"
```
这里只截取了部分报错信息，大体上就是说引擎是装了，但是还没有换上。通过搜索发现，在vimtex和latexmk的环境下，引擎是可以自定义更改的，只需要在.vimrc文件中加上以下的配置  
代码出处：https://tex.stackexchange.com/questions/392198/vimtex-and-xelatex
```vim
let g:vimtex_compiler_latexmk = { 
        \ 'executable' : 'latexmk',
        \ 'options' : [ 
        \   '-xelatex',
        \   '-file-line-error',
        \   '-synctex=1',
        \   '-interaction=nonstopmode',
        \ ],
        \}
```
然后重新测试和编译带有中文内容的.tex文件，测试成功！

# 参考资料
+ https://tex.stackexchange.com/questions/134365/installation-of-texlive-full-on-ubuntu-12-04
+ https://liam.page/2018/11/26/introduction-to-TeX-engine-format-and-distribution/
+ https://blog.csdn.net/qq_41814939/article/details/82288145
+ https://tex.stackexchange.com/questions/392198/vimtex-and-xelatex
