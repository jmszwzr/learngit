# Markdown语法学习


<!-- MDTOC maxdepth:6 firsth1:2 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [Markdown基础语法](#markdown基础语法)   
   - [换行](#换行)   
   - [空格](#空格)   
   - [加粗 | 斜体 | 斜体加粗](#加粗-斜体-斜体加粗)   
   - [Emoji](#emoji)   
   - [代码块](#代码块)   
   - [图片水平居中](#图片水平居中)   
- [表格](#表格)   
- [TeX公式](#tex公式)   
- [**资料阅读**](#资料阅读)   

<!-- /MDTOC -->

## Markdown基础语法
### 换行
	硬换行(soft break) Enter
	软换行(hard break) 空格+空格+Enter
### 空格
1. 在中文输入法的情况下：shift＋空格键，　之后再按空格键，那么空格键就会生效。　　
2. 使用　&nbsp；　这里的分号因为是中文输入法下的分号，所以没效，使用时不能使用中文分号。

### 加粗 | 斜体 | 斜体加粗
**加粗** 	_斜体_	*斜体*		***斜体加粗***
### Emoji
[Emoji Cheat Sheer](https://www.webpagefx.com/tools/emoji-cheat-sheet/)
:+1:

### 代码块
	首行一个Tab键 or 四个空格

### 图片水平居中
	<div align=center>
	![]()
	</div>

## 表格
|   Item   | Value | Qty |
|:-------- | -----:|:---:|
| Computer | $1600 |  5  |
| Phone    |   $12 | 12  |
| Pipe     |    $1 | 234 |

## TeX公式
$\Gamma(n) = (n-1)!\quad\forall n\in\mathbb N$

$$
\Gamma(z) = \int_0^\infty t^{z-1}e^{-t}dt\,.
$$



---------------

[1]: http://example.com/ "Optional Title"
## **资料阅读**

- [Markdown 语法说明 (简体中文版)](http://www.appinn.com/markdown/)
- [Markdown - 简单的世界](https://wizardforcel.gitbooks.io/markdown-simple-world/content/index.html)
- [Markdown 语法和 MWeb 写作使用说明](http://zh.mweb.im/markdown-syntax-guide-suggest-version-zh.html)