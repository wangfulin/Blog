# browserify简介

![browserify](https://camo.githubusercontent.com/e19e230a9371a44a2eeb484b83ff4fcf8c824cf7/687474703a2f2f737562737461636b2e6e65742f696d616765732f62726f777365726966795f6c6f676f2e706e67)
在浏览器中使用`require('modules')`

使用`node`的风格来组织浏览器端的代码并且可以加载通过`npm`安装的模块。

browerify将递归分析你应用中所有的`require()`调用去构建一个能够通过单个`<script>`标签引用的`js`文件。

## 如何开始
1. 安装
	> `npm install -g browserify`
2. 创建modules
	 - 创建foo.js
	 	> `module.exports=function(n){return n*111;}`
	 - 创建bar.js
	 	> `module.exports=function(s){return s+"!";}`
3. 创建main.js入口文件
		
		var foo=require("foo.js");
		var bar=require("bar.js");
		var str=foo(100)+bar("bar");
		console.log(str);
4. 使用browerify命令预编译main.js
	> browserify main.js > bundle.js

参考链接：

1. [node-browerify](https://github.com/substack/node-browserify)
2. [browserify-handbook](https://github.com/substack/browserify-handbook)