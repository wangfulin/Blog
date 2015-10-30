# Webpapck的简介
![webpack](https://camo.githubusercontent.com/ebc085019011ababb0d35024824304831c7dc72a/68747470733a2f2f7765627061636b2e6769746875622e696f2f6173736574732f6c6f676f2e706e67)
## 1，什么是webpack？
webpack是一个模块打包工具，主要作用是把浏览器中打包起来使用，然后它也能转换，打包，组合几乎所有的资源。

主要作用如下：
	1. 可以使用`CommonJs`和`AMD`模块加载（甚至同时使用）
	2. 可以创建一个整包或者几个区块进行运行时异步加载（减少初始加载时间）
	3. 在编译的时候解析依赖，减少运行时的大小
	4. 加载器可以在编译的时候预处理文件，比如把`coffescript`转换成`Javacript` ,`handlebars strings`转换成`compiled function`,`iamges`转换成`Base64`,等等
	5. 高度模块化的插件系统可以让你的应用做任何其他想做的事情



## 2，如何开始？
1. 创建一个文件夹，比如webpacktest
2. 创建文件entry.js
	>document.write("It works");
3. 创建文件index.html
	 
		<html>
			<head>
				<meta charset="utf-8" />
			</head>
			<body>
				<script type="text/javascript" src="bundle.js" charset="utf-8"></script>
			</body>
		</html>
4. 运行命令webpack ./entry.js bundle.js
## 参考链接
1. [github-webpack](https://github.com/webpack/webpack)
2. [webpack-github-io](http://webpack.github.io/)