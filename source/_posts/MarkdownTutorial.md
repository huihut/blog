---

title: Markdown 简易入门教程
subtitle: 自己整理的 Markdown 简介、编辑器推荐、语法、特征等，是 Markdown 的简易入门教程
date: 2017-01-25 1:45:50
author: huihut
tags:
	- Markdown
categories: 
	- CS
	
---

自己整理的 Markdown 简介、编辑器推荐、语法、特征等，是 Markdown 的简易入门教程

<!-- more -->


目录
=============

*   [概述](#overview)
	*	[简介](#summary)
	*	[官方文档](#doc)
	*	[Markdown编辑器](#editor)
   
*   [初级语法](#primary)
	*   [标题](#MarkdownHeader)
	*	[粗体和斜体](#bolditalic)
    *   [段落和换行](#paragraph)
    *   [分隔线](#hr)
    *   [引言](#blockquote)
    *	[列表](#list)
    	*   [无序列表](#disorderlist)
    	*   [有序列表](#sorderlist)
    *   [代码](#code)
    	*	[行内代码块](#linecode)
    	*	[段落代码块](#paragraphcode)
    *   [链接](#link)
    	*	[网址链接](#urllink)
    	*	[图片链接](#picturelink)
    		*	[指定图片宽高](#picturelinksize)
    		*	[用图床获取外链](#picturelinkchain)
   			
*   [进阶语法](#Advanced)
    *	[标签](#label)
	*	[目录](#content)
    *   [表格](#table)
    *	[脚注](#footnote)
    *	[公式](#formula)
    *	[流程图](#flowsheet)
    *	[序列图](#sequencemap)
    
*   [其他](#others)
    *   [兼容HTML](#html)
	*   [特殊字符自动转换](#autoescape)
    *   [反斜杠](#backslash)
    *   [自动链接](#autolink)
    
*   [感谢](#reference)

* * *

<h2 id="overview">概述</h2>

<h3 id="summary">简介</h3>

Markdown是一种轻量级标记语言，排版语法简洁，让人们更多地关注内容本身而非排版。它使用易读易写的纯文本格式编写文档，可与HTML混编，可导出 HTML、PDF 以及本身的 .md 格式的文件。因简洁、高效、易读、易写，Markdown被大量使用，如Github、Wikipedia等网站，如各大博客平台：WordPress、Drupal、简书等。

<h3 id="doc">官方文档</h3>


> [Markdown: Syntax](http://daringfireball.net/projects/markdown/syntax)
> 
> [Markdown 语法说明 (简体中文版)](http://wowubuntu.com/markdown/)


<h3 id="editor">Markdown编辑器</h3>

* 在线编辑器
	* [dillinger](http://dillinger.io/)——漂亮强大，支持md, html, pdf 文件导出。  
	
		![dillinger](http://www.williamlong.info/upload/4319_1.jpg)

	* [简书](http://www.jianshu.com/)——非常漂亮的博客平台，可以自动备份，直接拖入图片。  
	
		![简书](http://www.williamlong.info/upload/4319_3.jpg)
	
* Windows
	* [MarkdownPad](http://www.markdownpad.com/)——一款全功能的编辑器，被很多人称赞为windows 平台最好用的markdown编辑器。  
	
		![MarkdownPad](http://www.williamlong.info/upload/4319_10.jpg)
	
	* [MarkPad](http://code52.org/DownmarkerWPF/)——开源软件，可以直接在你的博客或者 GitHub 中打开、保存文档，直接将图片粘贴到 Markdown 文档中。  
	
		![MarkPad](http://code52.org/DownmarkerWPF/screenshot.png)
	
	* [Cmd Markdown](https://www.zybuluo.com/cmd/)——作业部落出品，全平台并支持Web端  	
	
		![Cmd Markdown](http://www.williamlong.info/upload/4319_6.jpg)
	
* Mac
	* [Mou](http://25.io/mou/)——简洁优雅，免费又好用，中文兼容性好。  
	
		![Mou](http://www.williamlong.info/upload/4319_14.jpg)
	
	* [Typora](https://typora.io/)——极致简洁，自定义皮肤。
	
		![Typora](http://www.williamlong.info/upload/4319_15.jpg)
	
	* [MacDown](https://macdown.uranusjr.com/)——简洁优雅，开源免费。
	  
		![MacDown](http://www.williamlong.info/upload/4319_16.jpg)
	
	* [Ulysses](https://www.ulyssesapp.com/)——文字写作推荐。  
	
		![Ulysses](http://www.williamlong.info/upload/4319_19.jpg)

* 多平台
	* [Atom](https://atom.io/)——github出的编辑器，支持各种编程语言，可装Markdown插件。
	
		![](http://www.williamlong.info/upload/4319_9.jpg)
	
	* [sublimetext](http://www.sublimetext.com/)——专业编辑器，支持各种编程语言。
	
		![](http://www.williamlong.info/upload/4319_8.jpg)


<h2 id="primary">初级语法</h2>


<h3 id="MarkdownHeader">标题</h3>


![标题](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/header.jpg)

Markdown 支持两种标题的语法，类 Atx 和类 Setext 形式。

* Atx（注意`#`后面有个空格）
	
		# 一级标题
		## 二级标题
		### 三级标题

* Setext（`-`与`=`数目任意，最好三个及以上，比较直观）

		一级标题
		======

		二级标题
		------

<h3 id="bolditalic">粗体和斜体</h3>

![bolditalic](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/bolditalic.jpg)

* 粗体

		**这是粗体**

		__这是粗体__


* 斜体

		*这是斜体*

		_这是斜体_

<h3 id="paragraph">段落和换行</h3>

![paragraph](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/paragraph.jpg)

* 第一种写法（上图的`这是第一段`），直接敲两个回车键即可
	
		这是第一段
		
		这是第二段
	
* 第二种写法（上图的`这是第二段`），在写完一段后敲两个空格，然后回车写下一段

		这是第二段  
		这是第三段

* 第三种写法（上图的`这是第三段`），在写完一段后用HTML的语法：`<br />`作为换行，然后写下一段

		这是第三段<br />这是第四段
		
		这是第三段<br />
		这是第四段

<h3 id="hr">分隔线</h3>

![hr](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/hr.jpg)

可以在一行中用三个及以上的星号、减号、等于号、底线来建立分隔线，行内不能有除空格外的其他东西，注意莫被打脸。 (≖ ‿ ≖)✧
	
	***
	---
	===
	___
	
	
	
<h3 id="blockquote">引言</h3>

![blockquote](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/blockquote.jpg)

	> 我想只用一个 “>” 号来写一个多行的引用，所以在扯鸡巴蛋地码字占空间，好像差不多了吧，嗯嗯~

	---

	> 还有一种写法就是每一行都用一个 “>” 号
	> 这样写比较美观一点

	---

	> > 另外一种就是嵌套引用，就像我一样，用两个“>”


<h3 id="list">列表</h3>

<h4 id="disorderlist">无序列表</h4>

![disorderlist](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/disorderlist.jpg)

无序列表可以在每行开头用星号、加号、减号来表示，也可以三者混合一起，推荐使用相同的字符，避免混乱。

	* 一朵百合花
	* 两朵百合花
	* 三朵百合花

<h4 id="sorderlist">有序列表</h4>

![orderlist](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/orderlist.jpg)

有序列表用数字接着一个英文句点来表示，数字可无序，但还是推荐使用`1. `、`2. `，避免混乱。

	1. 一朵百合花
	2. 两朵百合花
	3. 三朵百合花


<h3 id="code">代码</h3>

<h4 id="linecode">行内代码块</h4>

![linecode](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/linecode.jpg)

	
	I am a `code`
	I am a `` ` ``
	

<h4 id="paragraphcode">段落代码块</h4>

![paragraphcode](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/paragraphcode.jpg)

	#### 第一种
	
		int main()
		{
			printf("我是个段落代码块");
			return 0;
		}
	
另外，可以用三个反引号和语言名，作为标记代码所使用的语言

![paragraphcode2](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/paragraphcode2.jpg)

我的 Mou 编辑器不能识别 (ノ▼Д▼)ノ


<h3 id="link">链接</h3>

<h4 id="urllink">网址链接</h4>

![urllink](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/urllink.jpg)  

网址链接有两种形式：**行内式**、**参考式**。

不管是哪一种，链接文字都是用 [方括号] 来标记，双引号`""`的`title`可写可不写。

* 行内式
	
		[huihut](https://huihut.github.io/)
		
		[huihut](https://huihut.github.io/ "huihut")
		
		[huihut](https://huihut.github.io/ 'huihut')

* 参考式

	* 一般写法
	
	
			[huihut][1]
			[1]: https://huihut.github.io/


* 隐式链接标记——可省略id，只需要[text]与下面[方括号]内容相同即可
	
	
			[Google][]
			[Google]: http://google.com/

* 拓展
		
	* 这里的链接辨别标签可以有字母、数字、空白和标点符号，但是并不区分大小写，因此下面两个链接是一样的：
		
				[text][a]
				[text][A]
			
	* 链接 title 可以用双引号、单引号、圆括号包起来，因此，下面这三种链接的定义都是相同：
	
				[1]: https://huihut.github.io/  "title"
				[1]: https://huihut.github.io/  'title'
				[1]: https://huihut.github.io/  (title)
			
**特别注意**：Markdown.pl 1.0.1 会忽略单引号包起来的链接 title
	
    	
<h4 id="picturelink">图片链接</h4>

![picturelink](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/picturelink.jpg) 

图片链接与上面的网址链接类似，同样有两种形式：行内式和参考式，只不过图片链接在前面加上一个感叹号`!`，在此不做累述。

* 行内式

		![huihut](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/H_ya.png)

* 参考式

		![谷歌][1]
		[1]: https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_120x44dp.png "Google"
    	
    	
<h5 id="picturelinksize">指定图片宽高</h5>

* Markdown 一般不支持指定图片的宽高，若要指定宽高可以使用普通的 `<img>` 标签
	 	
		<img src="./xxx.png" width = "100" height = "100" alt="title" align=center />
    	 
    如果需要居中可以在外围包围`div`标签
    	
    	<div  align="center">    
		<img src="xxx.png" width = "100" height = "100" alt="title" align=center />
		</div>
			
* 使用支持指定图片大小的 Markdown 编辑器，如 Mou
		
		![](xxx.png =100x100)
			
			
<h5 id="picturelinkchain">用图床获取外链</h5>

网上有许多图床，这里推荐两个 **七牛图床** 和 **极简图床**。

* [七牛图床](https://www.qiniu.com/) 

	<img src="http://huihut-img.oss-cn-shenzhen.aliyuncs.com/qiniutuchuang.jpg" width = "90%" height = "90%"/>

* [极简图床](https://yotuku.cn/)

	<img src="http://huihut-img.oss-cn-shenzhen.aliyuncs.com/jijiantuchuang.jpg" width = "90%" height = "90%"/>


<h2 id="Advanced">进阶语法</h2>

<h3 id="label">标签</h3>

![label](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/label.jpg)

* 方法一

		title: Markdown 简易入门教程
		date: 2017-01-25 1:45:50
		tags: Markdown
		categories: 技术

* 方法三

		tags:
		- Markdown
		- 语言
		categories:
		- 技术

* 方法三

		tags: [Markdown,语言]
		categories: [技术]
		

<h3 id="content">目录</h3>

* 方法一

	![](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/content.jpg)

	这种目录其实是用 **HTML** 加 **Markdown的链接** 实现，分为两个部分，**目录部分**和**标题部分**。

	* 目录部分——实质是链接，链接的`[地址]`填需要跳转到的标题的`id`属性（自定义）。

			[跳到标题一](#title1)
	
	* 标题部分——实质是HTML的标题标签，标签里面的`id`属性等于待跳转的目录的`[地址]`。

			<h1 id="title1">标题一</h1>
		
	
* 方法二
	
	![content2](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/content2.jpg)
		
	这种方法非常简单，就是直接添加 `[TOC]`，标题1~6样式的内容会被提取出来作为目录，然而有些编辑器不能使用这功能，如 Mou 不能使用。我是在有道云笔记的 Markdown 中截图的。
	
		[TOC]
		
		# 标题一
		……
		## 标题二
		……
		### 标题三

	这里有个jQuery插件，貌似可以让Markdown生成目录：
	
	[https://github.com/i5ting/i5ting_ztree_toc](https://github.com/i5ting/i5ting_ztree_toc)


<h3 id="table">表格</h3>

![table](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/table.jpg)

* 表格一般这样子写，这应该是最简单的写法了

		id    |   name   |   score
		---   |   ---    |   ---
		001   |   Mark   |   90
		002   |   Ford   |   80
		003   |   Alan   |   95

* 还有就是对齐了，用`:`对齐，`:`写在在`---`的左边就是左对齐，右边就是右对齐，两边都写就是居中。

		|long_long_id|long_long_name|long_long_score|
		|    ---     |    :---:     |     ---:      |
		|    001     |     Mark     |      90       |
		|    002     |     Ford     |      80       |
		|    003     |     Alan     |      95       |
		

<h3 id="footnote">脚注</h3>

![footnote](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/footnote.jpg)

	这是脚注一[^1]
	
	[^1]: 脚注一


<h3 id="formula">公式</h3>

![formula](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/formula.jpg)

* 方法一：使用Google Chart

		<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" style="border:none;">

* 方法二：使用forkosh

		<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" style="border:none;">
 
* 方法三：使用codecogs
  
		<a href="https://www.codecogs.com/eqnedit.php?latex=x=\frac{-b\pm&space;\sqrt{b^{2}-4ac}}{2a}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?x=\frac{-b\pm&space;\sqrt{b^{2}-4ac}}{2a}" title="x=\frac{-b\pm \sqrt{b^{2}-4ac}}{2a}" /></a>

* 方法四：使用MathJax引擎——先加载脚本`<script>`，后解析公式。

		<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
		
		$$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$$


<h3 id="flowsheet">流程图</h3>

![flowsheet](http://cdn2.wiz.cn/wp-content/uploads/2015/11/QQ20151123-0.png)

像流程图这种复杂的功能不推荐在 Markdown 中使用，因为很多编辑器都不支持，我使用了几个编辑器都不能生成流程图，所以上图是在为知笔记官方 Markdown 新手指南中找到的。

![flowsheetcode](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/flowsheetcode.jpg)

	```flow
	st=>start: Start
	e=>end: End
	op1=>operation: My Operation
	sub1=>subroutine: My Subroutine
	cond=>condition: Yes or No?
	io=>inputoutput: catch something...
	st->op1->cond
	cond(yes)->io->e
	cond(no)->sub1(right)->op1
	```
	
更多关于流程图的语法说明：

[https://github.com/adrai/flowchart.js](https://github.com/adrai/flowchart.js)


<h3 id="sequencemap">序列图</h3>

![sequencemap](http://cdn2.wiz.cn/wp-content/uploads/2015/11/QQ20151123-1.png)

![sequencemapcode](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/sequencemapcode.jpg)
	
	```sequence
	Alice->Bob: Hello Bob, how are you?
	Note right of Bob: Bob thinks
	Bob-->Alice: I am good thanks!
	```

更多关于时序图的语法说明：

[https://github.com/bramp/js-sequence-diagrams](https://github.com/bramp/js-sequence-diagrams)


<h2 id="others">其他语法</h2>

<h3 id="html">兼容HTML</h3>

![html](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/html.jpg)

Markdown 可以和 HTML 混编，甚至可以完全用 HTML 语法来写。要注意在 HTML 标签中的 Markdown 代码是不起作用的。

	<font color='blue' style='font-size:30px'>蓝色</font>
	
	<div>
	# HTML 标签里面的 Markdown 语法不起作用
	**你看我没有变粗**
	</div>

<h3 id="autoescape">特殊字符自动转换</h3>

* HTML 语法——在 HTML 中所有`<`和`&`都要转换，包括链接（URL）

	* 用 `&lt;` 表示 `<`——起始标签
	* 用 `&amp;` 表示 `&` ——标记 HTML 实体

* Markdown 语法——Markdown 则会自动转换

<h3 id="backslash">反斜杠</h3>

Markdown 可以利用反斜杠来插入一些在语法中有其它意义的符号。如：

\*literal asterisks\*

可用

	\*literal asterisks\*

Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：

	\   反斜线
	`   反引号
	*   星号
	_   底线
	{}  花括号
	[]  方括号
	()  括弧
	#   井字号
	+   加号
	-   减号
	.   英文句点
	!   惊叹号



<h3 id="autolink">自动链接</h3>
    
![autolink](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/autolink.jpg)

* 网址自动链接

		<https://huihut.github.io/>

* 电子邮件自动链接

		<huihut@outlook.com>
    

<h2 id="reference">感谢</h2>

> [Markdown: Syntax](http://daringfireball.net/projects/markdown/syntax)
>
> [Markdown 语法说明 (简体中文版)](http://wowubuntu.com/markdown/)
>
> [Markdown——入门指南](http://www.jianshu.com/p/1e402922ee32/)
>
> [Markdown语法手册](https://www.zybuluo.com/xxliixin1993/note/125827)
> 
> [好用的Markdown编辑器一览](http://www.williamlong.info/archives/4319.html)
>
> [markdown中插入图片怎么定义图片的大小或比例？](https://www.zhihu.com/question/23378396)
>
> [Markdown进阶语法整理](http://www.jianshu.com/p/0b257de21eb5)
>
> [为知笔记 Markdown 新手指南](http://www.wiz.cn/feature-markdown.html)
>
> [Markdown中插入数学公式的方法](http://blog.csdn.net/xiahouzuoxin/article/details/26478179)
>
> [i5ting/i5ting_ztree_toc](https://github.com/i5ting/i5ting_ztree_toc)
>
> [flowchart.js](http://flowchart.js.org/)
>
> [adrai/flowchart.js](https://github.com/adrai/flowchart.js)
>
> [js-sequence-diagrams](https://bramp.github.io/js-sequence-diagrams/)
>
> [bramp/js-sequence-diagrams](https://github.com/bramp/js-sequence-diagrams)
>