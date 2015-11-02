# 使用负边距权威指南
> **原文:** [The Definitive Guide to Using Negative Margins](http://www.smashingmagazine.com/2009/07/the-definitive-guide-to-using-negative-margins/)

自从1998年CSS2作为推荐以来，表格的时候渐渐退去，成为历史。正因为此，从那以后CSS布局成为了优雅代码的代名词。

对于所有设计师使用过的CSS概念，负边距作为最少讨论到的定位方式要记上一功。这就像是在线纹身-每个人都会做，但是没有人会谈论它。（It’s like an online taboo—everyone’s doing it, yet no one wants to talk about it.）

## 为其正名

我们都使用过CSS得外边距，但是当谈到负边距的时候，我们好像往差的方向发展啦。在网页设计中负边距的使用出现了两种极端，一种特别喜欢它，也有一些人认为这完全就是魔鬼的作品。

负边距的使用如下：

	#content {margin-left:-100px; }

负边距通常在小范围使用。但是接下来你会看到，它能做的事情很多。下面是一些你应该知道的关于负边距的事情：

- 他们是完全有效的CSS
> 这不是在跟你开玩笑。W3C甚至都说，在外边框中使用负边距是允许的。要了解更多可以点击这边[文章](http://www.w3.org/TR/CSS2/box.html#margin-properties)

- 负边距不是在hack
> 这是尤其正确的。正是因为没有很好地了解负边距才是导致各种奇怪的问题。只有在被用来解决其他地方的bug的时候才是hack

- 它符合正常的文档流
> 当负边距使用在没有浮动的元素上时并不会破坏正常的文档流。所以付过你使用负边距把元素向上微调的话，所有后面的元素也会向上微调。

- 它是相当好的兼容性
> 负边距基本上被所有现代的浏览器支持（IE6的大部分情况也是）

- 当使用了float之后，会有不同的表现
> 负边距不是你平常使用的属性，所以使用的时候要格外小心。

- Dreamweaver不理解它
> 负边距不会在DW的设计窗口展示出效果。那你为什么还用DW的设计窗口查看效果呢？

## 与其共事

负边距如果可以正确的使用的话它的功能是很强大的。有两种场景负边距是很重要的。

### 在static元素中使用负边距

![static elements using negative margins](/imgs/definitive-guide-to-negative-margin/margin-motion.gif)

一个static元素是一个没有使用过float的元素。上面的图片展示了一个static的元素使用负边距之后的情况。

- 当一个static元素在top/left使用负边距时，它把元素向这个特定的方向拉，比如

		/* Moves the element 10px upwards */
		#mydiv1 {margin-top:-10px;}

- 但是当你将负边距设置为相对bottom/right时，它并不会把元素向下或右拉，相反，它会把后面的元素往里面拉，从而覆盖自己。
		
		/*
		* 所有在#mydiv1后面的元素都会向上
		* 移动10px，而#mydiv1一点都不会移动
		*/
		#mydiv1{margin-bottom:-10px;}

- 如果宽度没有设置，左右负边距会把元素向两个方向拉以增加宽度。在这里margin的作用相当于padding

### 在浮动中使用负边距

加入下面就是我们的html代码：

	<div id="mydiv1">First</div>
	<div id="mydiv2">Second</div>

- 如果对一个浮动的元素使用负边距，它会产生一个空白，其他元素就可以覆盖这一部分。这个技巧可以很好地用户流式布局。比如有一列宽度100%，另一列有固定的宽度，比如说100px。

		/* 
		A negative margin is applied opposite the float 
		*/
		#mydiv1 {float:left; margin-right:-100px;}
- 如果两个元素都使用了左浮动并且设置margin-right:-20px。#mydiv2会把#mydiv1看成宽度缩小20px（所以会覆盖一部分），但是有趣的是#mydiv1并不会有任何变化，而是依然保持原先的宽度。

- 如果负边距和宽度一样大的话，它就会被完全覆盖掉。因为外边距，内边距，边框和内容加起来等于元素的宽度。如果负外边距等于元素的宽度的话，那么该元素的宽度就会变成0px。

## 学以致用

既然我们知道使用负边距在CSS2中是有效的，使用它可以给我们提供一些非常有趣的CSS技巧。

### 把单个列表变成三列

![splitting a list into columns](/imgs/definitive-guide-to-negative-margin/splitting-a-list.gif)

如果你有一个列表垂直方向太长了，为什么不把它分成几列呢？负边距可以让你在不增加任何浮动和标签的情况下完成。你会发现用负边距实现这个是多么地简单，就像下面：

HTML
	
	<ul> 
		<li class="col1">Eggs</li>
		<li class="col1">Ham<li> 
		<li class="col2 top">Bread<li> 
		<li class="col2">Butter<li> 
		<li class="col3 top">Flour<li> 
		<li class="col3">Cream</li> 
	</ul>

CSS

	ul {list-style:none;}
	li {line-height:1.3em;}
	.col2 {margin-left:100px;}
	.col3 {margin-left:200px;}
	.top {margin-top:-2.6em;} /* the clincher */

通过对.top的添加margin-top:-2.6em。所有的元素会完美的对齐好。使用负边距会比使用相对定位好很多，因为你只需要给新的一列的第一个元素添加负边距即可。酷吧，哈哈哈

### 重叠来强调

![overlap for added emphasis](/imgs/definitive-guide-to-negative-margin/phlashers.gif)

故意重叠元素也是一种很好地设计隐喻。重叠效果可以增强深度感从而为突出特定元素。一个很好地例子就像上图一样，通过重叠来吸引注意力。只需要使用z-index属性和一点小创意你就可以做到。

### 惊艳的3D文本效果

![smashing 3D text effects](/imgs/definitive-guide-to-negative-margin/nike.jpg)

这是一个精致的技巧。通过使用两个视图的两种颜色创建safari一样有点倾斜的效果。然后通过负边距来把其中一个叠加到另一个上面，保持1到2像素的偏移。这样你就可以二道可选的，机器友好的倾斜字体。就不需要浪费很多贷款来加载大的图片来实现这个效果啦

### 简单的两列布局

负边距也是在流式布局中创建简单一列宽度固定，一列内容为宽度的100%的两列布局的好方法。

HTML
	
	<div id="content"> <p>Main content in here</p> </div> 
	<div id="sidebar"> <p>I’m the Sidebar! </p> </div>

CSS

	#content {width:100%; float:left; margin-right:-200px;}
	#sidebar {width:200px; float:left;}

哈哈，这样你就得到了一个简单的两列布局。它也能在IE6完美的渲染出来。现在为了让#sidebar不要被#content给掩盖，只要简单的加上：
	
	/* Prevent text from being overlapped */

	#content p {margin-right:210px;}

	/* It’s 200px + 10px, the 10px being an additional margin.*/

当适当的使用的时候，负外边距能够提供一个灵活的文档结构，完爆table的布局。灵活的文档布局是一种可访问性和SEO的技巧，通过它能够让你根据你的关注点以任意顺序组织你的html代码。这里有一个[文章](http://www.severnsolutions.co.uk/twblog/archive/2004/07/01/cssnegativemarginsalgebra)讨论了负边距在多列布局中的应用。

### 微调元素

这是负外边距最常也是最简单的使用方式。假如你把第十个div插入到9个其他的div中，不知道什么原因没有正确的排列，使用负边距来调整这个div就不需要改变其他9个div了，很方便。

## 解决bug

### 文本和链接问题

在float中使用负边距可能会在旧的浏览器造成一些问题，比如下面的这些：

- 让链接不可点击
- 文本变得很难选择
- 失去焦点的时候按tab键失效

**解决方法：**只要添加position:relative，就可以啦。

###　图片被剪切啦

如果你运气不好刚好在办公室使用IE6，当遇到覆盖和浮动的时候内容有些时候回突然被剪切掉。

**解决方法：**同样的只要给浮动元素加上position:relative，所有的问题就解决啦。

## 结论

负外边距能够在不使用任何额外标签的情况下定位元素让它在现在网页设计中占有一席之地。随着更多的用户使用更新的浏览器（包括IE8)，未来使用这些技巧的网站会变得更加有前景。

##　资源链接

更多关于负边距的信息可以在看下面的链接：

- [Search This](http://www.search-this.com/2007/08/01/the-positive-side-of-negative-margins/)
- [Severn Solutions](http://www.severnsolutions.co.uk/twblog/archive/2004/07/01/cssnegativemarginsalgebra)


























































