# npm常用命令解析
1. npm install
	>npm install < packagename >,比如npm install jquery
	
	>npm install < packagename >@< version >,比如npm install jquery@2.1.3

	>npm install < packagename >@< version-range >,比如 npm install jquery@">=1.0.1 <3.0.0"

	>npm install < packagename >@< tagname>,比如npm install jquery@latest

	>npm install git地址,比如 npm install git@github.com:jquery/jquery

	>npm install还可以加一些参数比如-g,--save,--save-dev，分别表示全局安装，将依赖添加到package.json的dependencies，添加到devDependencies。
2. npm remove <package-name>
3. npm update <package-name>
4. npm config
	* npm config set，比如npm config set registry http://registry.cnpmjs.org
	* npm config get，比如npm config get registry
	* npm config list
	* npm config ls -l,显示所有的默认设置
	* npm config edit，更改配置文件
5. npm init，可以通过命令行来初始化package.json文件
4. npm list，显示所有安装的包
	- npm list -g 全局安装的模块
	- npm list -g --depth=0 不显示模块的依赖
5. npm root 查看当前包的安装路径
6. npm root -g 查看全局包的安装入境
7. npm [packagename] help,获取帮助
8. npm outdated，查看所有过时的包
9. npm prune，移除不再依赖的包
10. npm search,查找相应的包
11. npm cache clean,删除cache,即npm config get cache这个文件夹存的缓存。