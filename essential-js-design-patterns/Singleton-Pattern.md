# 单例模式

> **原文：**[The Single Pattern](http://addyosmani.com/resources/essentialjsdesignpatterns/book/#singletonpatternjavascript)

单例模式被熟知是因为它把一个类的实例化限制在只有一个对象。传统的实现方式是：创建一个类，这个类里面有一个方法在对象不存在的时候创造一个实例，存在的时候，只需要返回这个对象的引用即可。

单例和静态类（或者对象）是有区别的，因为我们可以延迟他们的初始化。因为他们需要一些在初始化的时候不能获得的信息。他们没有提供一种方式让不知道他们之前引用的代码去获取他们。这是因为由单例返回的既不是对象也不是类，而是一种结构。想想大括号形成的不是真正的闭包，函数作用于提供的闭包才是真正的闭包。

Javascript中，单例作为共享资源的命名空间，它隔离了实现的代码和全局变量，为了能够让函数有唯一的一个入口。

我们可以通过下面的方法实现单例：

	var mySingleton=(function(){
		//存储单例引用的实例
		var instance;

		function init(){
			//单例

			//私有方法和属性
			function privateMethod(){
				console.log("I am private);
			}

			var privateVariable="I'm alse private";
	
			var privateRandomNumber=Math.random();

			return {
				//公有方法和属性
				publicMethod:function(){
					console.log("The public can see me!);
				},
				publicProperty:"I am also public",
				getRandomNumber:function(){
					return privateRandomNumber;
				}
			};
		}

		return {

			//如果存在获取单例实例的引用
			//不存在，创建实例
			getInstance:function(){
				if(!instance){
					instance=init();
				}
				return instance;
			}
		};
		
	})();

	var myBadSingleton=(function(){
		//指向单例的引用
		var instance;

		function init(){
			//单例
			var privateRandomNumber=Math.random();

			return {
				getRandomNumber:function(){
					return privateRandomNumber;
				}
			}
		}

		return {
			//无论什么时候都创建一个单例实例
			getInstance:function(){
				instance=init();
	
				return instance;
			}
		};
	})();

	//使用：
	
	var singleA=mySingleton.getInstance();
	var singleB=mySingleton.getInstance();
	console.log(singA.getRandomNumber()===singleB.getRandomNumber()); //true

	var badSingleA=myBadSingleton.getInstance();
	var badSingleB=myBadSingleton.getInstance();
	console.log(badSingleA.getRandomNumber()!==badSingleB.getRandomNumber());//true
	
	//注意：因为我们使用的是随机数
	//所以上面的值还是有可能相同的
	//但是可能性很低，上面的例子还是有效的。

使用单例可以让我们有一个指向实例的统一入口（一般通过`MySingleton.getInstance()`的方式），这样我们就不需要直接通过`new MySingleton()`的方式直接调用啦。这在javascript中也是可以实现的。

在四人帮这本书中，单例的适用场景被描述为下面这些：

- 必须只有一个类的实例，而且必须可以通过大家都知道的入口让大家可以访问到。
 
- 当这个唯一的实例需要被之类扩展的时候，用户可以在不需要修改代码的情况下扩展它。

第二点指出了我们可能遇到的场景，比如：

	mySingleton.getInstance=function(){
		if(this._instance==null){
			if(isFoo()){
				this._instance=new FooSingleton();
			}else{
				this._instance=new BasicSingleton();
			}
		}
		return this._instance;
	}

这里，`getInstance`变得有些像工厂方法，我们访问它的时候并不需要更新我们的每一部分代码。上面的`FooSingleton`可能是`BasicSingleton`的之类，并且实现了同样的接口。

为什么在单例中延迟执行被认为很重要呢？：

> 在C++中，它被用来让我们解决动态初始化执行顺序的不可预测性，把控制权交给了程序。

需要注意的是区分类的静态实例和单例的区别是很重要的。尽管单例可以被实现成静态实例。但是同时也可以延迟构造，不需要消耗资源也不会消耗内存直到需要它的时候再初始化。

如果我们有一个可以被直接初始化的静态对象，我们需要确保代码是按照顺序执行的。（比如，车的对象初始化的时候车轮必须已经存在）并且它在你有很多资源文件的时候不会变大。

单例和静态对象都很有用，但是不能过度使用。就像不能过度使用其他模式一样。

实践中，当我们在整个系统中只需要一个对象与其他对象通信的时候，单例模式是非常有用的。下面就是单例在上下文中使用模式的一个例子：

	var SingletonTester = (function () {
 
  	// options: an object containing configuration options for the singleton
  	// e.g var options = { name: "test", pointX: 5};
 	 function Singleton( options ) {
 
    // set options to the options supplied
    // or an empty object if none are provided
    options = options || {};
 
    // set some properties for our singleton
    this.name = "SingletonTester";
 
    this.pointX = options.pointX || 6;
 
    this.pointY = options.pointY || 10;
 
  	 }
 
  	 // our instance holder
  	var instance;
 
  	// an emulation of static variables and methods
  	var _static = {
 
    name: "SingletonTester",
 
   	 // Method for getting an instance. It returns
   	 // a singleton instance of a singleton object
   	 getInstance: function( options ) {
     	 if( instance === undefined ) {
      	  instance = new Singleton( options );
      	}
 
     	 return instance;
 
    	}
 	 };
 
 	 return _static;
 
	})();
 
	var singletonTest = SingletonTester.getInstance({
  		pointX: 5
	});
 
	// Log the output of pointX just to verify it is correct
	// Outputs: 5
	console.log( singletonTest.pointX );

尽管单例在这里使用时有效的，但是经常当我们需要在javascript中使用它的时候，往往意味着我们应该重新审视我们的设计啦。

他们往往意味着系统的模块要不就是耦合过紧啦，要么逻辑延伸的太大啦。单例的使用往往会让测试变得更加困难，因为存在隐藏依赖，难以创建多个实例，难以找到根依赖等问题。



	
