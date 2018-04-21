---
title: "Markdown语法"
date: 2018-04-17T11:13:47+08:00
draft: false
---



段落
---
一个段落是由一个以上的连接的行句组成，而一个以上的空行则会划分出不同的段落（空行的定义是显示上看起来像是空行，就被视为空行，例如有一行只有空白和 tab，那该行也会被视为空行），一般的段落不需要用空白或tab缩进。

区块引用
---
区块引用则使用 email 形式的 '>' 角括号

	> This is a blockquote.
	> 
	> This is the second paragraph in the blockquote.
	>
	> ## This is an H2 in a blockquote

标题
---
Markdown 支持两种标题的语法，Setext 和 atx 形式。Setext 形式是用底线的形式，利用 = （最高阶标题）和 - （第二阶标题），Atx 形式在行首插入 1 到 6 个 # ，对应到标题 1 到 6 阶

	一阶标题
	===
	二阶标题
	---

	or

	#一阶标题
	##二阶标题

斜体
---

	*一个星号，表示斜体*

加粗
---

	**两个星号，表示加粗**

删除线
---

	~~这样子表示删除~~

无序列表
---
无序列表使用星号、加号和减号来做为列表的项目标记，这些符号是都可以使用的

	* Candy.
	* Gum.
	* Booze.

	or

	+ Candy.
	+ Gum.
	+ Booze.

	or

	- Candy.
	- Gum.
	- Booze.

有序列表
---
有序的列表则是使用一般的数字接着一个英文句点作为项目标记

	1. Red
	2. Green
	3. Blue

列表中的段落
---
如果你在项目之间插入空行，那项目的内容会用 `<p>` 包起来，你也可以在一个项目内放上多个段落，只要在它前面缩排 4 个空白或 1 个 tab 。

	* A list item.

	    With multiple paragraphs.

	* Another item in the list.

链接
---
Markdown 支援两种形式的链接语法： 行内 和 参考 两种形式，两种都是使用角括号来把文字转成连结。

行内形式是直接在后面用括号直接接上链接：

	This is an [example link](http://example.com/).

你也可以选择性的加上 title 属性：

	This is an [example link](http://example.com/ "With a Title").

参考形式的链接让你可以为链接定一个名称，之后你可以在文件的其他地方定义该链接的内容：

	I get 10 times more traffic from [Google][1] than from
	[Yahoo][2] or [MSN][3].

	[1]: http://google.com/ "Google"
	[2]: http://search.yahoo.com/ "Yahoo Search"
	[3]: http://search.msn.com/ "MSN Search"

title 属性是选择性的，链接名称可以用字母、数字和空格，但是不分大小写：

	I start my morning with a cup of coffee and
	[The New York Times][NY Times].

	[ny times]: http://www.nytimes.com/

图片
---
图片的语法和链接很像。

行内形式（title 是选择性的）：

	![alt text](/path/to/img.jpg "Title")

参考形式：

	![alt text][id]

	[id]: /path/to/img.jpg "Title"

代码
---
在一般的段落文字中，你可以使用反引号 ` 来标记代码区段

	I strongly recommend against using any `<blink>` tags.

如果要建立一个已经格式化好的代码区块，只要每行都缩进 4 个空格或是一个 tab 就可以了

换行
---

先编辑一行文字，编辑好一行文字后敲两个空格，再按回车键编辑另一行文字