# npm常用命令解析
1. npm install
	>npm install < packagename >,比如npm install jquery
	
	>npm install < packagename >@< version >,比如npm install jquery@2.1.3

	>npm install < packagename >@< version-range >,比如 npm install jquery@>=1.0.1

	>npm install < packagename >@< tagname>,比如npm install jquery

	>npm install git地址
2. npm remove <package-name>
3. npm update <package-name>
4. npm config
	* npm config set registry http://registry.cnpmjs.org
	* npm config get registry
	* npm config list
	* npm config edit
5. npm init
4. npm list
5. npm root 查看当前包的安装路径
6. npm root -g 查看全局包的安装入境
7. npm help
8. npm outdated
9. 