# Web字体的初探
   
### 一，字体基本概念的介绍

#### 1.1 字体的分类
##### 1.1.1 Serif（衬线体）
	Serif（衬线）：在印刷的文字中衬线字体对于人眼的辨识更轻松，阅读更舒服横细竖粗，开始和结束的地方有装饰。在web上的字体，衬线字体比无衬线字体的辨识度更低，因为屏幕像素有限，不能很好地渲染出衬线体的效果。
##### 1.1.2 Sans-Serif（无衬线体)
	Sans-Serif(无衬线体）：在印刷的文字中，无衬线体比较醒目，在小字体场合比衬线体更加清晰，但是辨识度没有衬线体高。在web字体中，无衬线字体比衬线字体更易读。
##### 1.1.3 Monospace（等宽字体）
	Monospace（等宽字体）：每个宽度都一致的字体，看起来比较整齐，比较适合用于显示代码。比较著名的有Courier New字体。
##### 1.1.4 Cursive（草书）
	Cursive（草书）：相当于印刷中的手写体，看起来比较流畅，像手写一样。
#### 1.2 常用的字体
	Serif，Sans-Serif，Monospace属于标准字体，Cursive,Fantasy属于非标准字体
##### 1.2.1 衬线体（Serif）
	常用的中文衬线体：宋体（Simsun），仿宋（FangSong），楷体（KaiTi），华文仿宋（STFangSong），华文楷体（STKaiTi）。
	常见的英文衬线体：Times new Roman，Times
##### 1.2.2 无衬线体（Sans-Serif）
	常见的中文无衬线体：微软雅黑（Microsoft YaHei），黑体（SimHei），华文细黑（STXiHei）
	常见的英文无衬线体：Tahoma，Arial，Helvetica，Verdana
##### 1.2.3 等宽字体（monospace）
	常见的等宽字体：Courier New，Courier
##### 1.2.4 草书（Cursive）
	常见的草书：Comic Sans MS
##### 1.2.5 Fantasy
	常见的Fantasy：Impact

### 二，网站中使用的字体
#### 2.1 英文网站中使用的默认字体
	font：12px/1.5 Tahoma,Helvetica,Arial,sans-serif
	Tahoma：英文windows操作系统默认的字体
	Helvetica：Mac OS X系统的系统默认字体
	Arial：早期windows英文系统的默认字体。XP和Vista都是Tahoma
	Sans-serif是针对linux的，linux默认只有kernel，字体由用户自定义。
	无论是XP还是Vista下，不指定网页的中文字体时，默认的就是宋体。因此在font-family中使用”宋体“是多余的，可以省去。
#### 2.2 Windows操作系统提供的中文字体
	黑体：SimHei
	宋体：SimSun
	新宋体：NSimSun
	仿宋：FangSong
	楷体：KaiTi
	仿宋GB2312：FangSongGB2312
	楷体GB2312：KaiTiGB2312
	微软雅黑：Microsoft YaHei(Windows 7开始提供）
#### 2.3 OS X操作系统提供的中文字体
	冬青黑体：Hiragino Sans GB（Snow Leopard开始提供）
	华文细黑：STHeiti Light（又名STXiHei）
	华文黑体：STHeiTi
	华文楷体：STKaiTi
	华文宋体：STSong
	华文仿宋：STFangsong
#### 2.4 更多有趣的字体使用
上面介绍的字体属于常见的字体，也就是我们所说的Web safe font。其在大部分网站是可以正常显示的。下面介绍的是比较有趣特殊的字体的使用方式。
#### 2.5 使用web font的方法
##### 2.5.1 使用link标签
	通过link导入样式，然后直接通过font-family使用，如：
`<link href='https://fonts.googleapis.com/css?family=Lobster' rel='stylesheet' type='text/css'>`
`font-family:"lobster"`

参考[Google Fonts](https://www.google.com/fonts)
##### 2.5.2 使用@import导入
	通过@import导入字体的样式，如：
`@import url(https://fonts.googleapis.com/css?family=Candal);`
`font-family:"Candal"`

参考[Google Fonts](https://www.google.com/fonts)
##### 2.5.3 使用javascript
	通过javascript获取字体样式，如：
`<script type="text/javascript">
			  WebFontConfig = {
				google: { families: [ 'Shadows+Into+Light::latin' ] }
			  };
			  (function() {
				var wf = document.createElement('script');
				wf.src = ('https:' == document.location.protocol ? 'https' : 'http') +
				  '://ajax.googleapis.com/ajax/libs/webfont/1/webfont.js';
				wf.type = 'text/javascript';
				wf.async = 'true';
				var s = document.getElementsByTagName('script')[0];
				s.parentNode.insertBefore(wf, s);
			  })(); 
		</script>
`
`font-family:"Shadows Into Light"`

参考[Google Fonts](https://www.google.com/fonts)

##### 2.5.4 使用font-face
	首先需要从网站下载对应的字体，然后url填入文件路径，如：
`@font-face{
				font-family:"saucer";
				src:url("fonts/SaucerBB.ttf") format("truetype");
			}`
`font-family:saucer;`

##### 2.5.5 使用这些特殊字体的弊端
	使用这些特殊字体可以产生很炫酷的文字，但是也存在很大的弊端：
	1，不同的环境显示的内容可能不一样
	2，显示的内容不可靠
	3，需要把字体包含到网站（有时可能有100kb大）中需要消耗大量的下载时间

### 三，大型网站上的字体实践
	1.淘宝：font-family: tahoma, arial, 'Hiragino Sans GB', 宋体, sans-serif;
	2.百度：font-family: arial, 宋体, 'Hiragino Sans GB', 'Microsoft Yahei', 微软雅黑, 宋体, Tahoma, Arial, Helvetica, STHeiti;
	3.京东：font-family: Arial, Verdana, 宋体；
	4.Youtube：font-family: Roboto, arial, sans-serif;
	5.github：font-family: Helvetica, arial, nimbussansl, liberationsans, freesans, clean, sans-serif, 'Segoe UI Emoji', 'Segoe UI Symbol';
参考的网站：

[creating good websites](http://www.leafdigital.com/class/lessons/graphicdesign1/4.html)

[serif和sans-serif的区别](http://kb.cnblogs.com/page/192018/)

[中文字体网页开发指南](http://www.ruanyifeng.com/blog/2014/07/chinese_fonts.html)
