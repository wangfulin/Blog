# jQuery事件绑定的最佳实践

> **原文：**[Effective Event Binding with jQuery](http://www.sitepoint.com/effective-event-binding-jquery/)

如果你经常使用jQuery，那么你也许很熟悉事件绑定。这是很基本的东西，但是深入一点，你就能够找到机会让你事件驱动的代码变得不太零碎，并且更容易管理。

## 更好的选择器策略

让我们从基础的例子开始。下面的HTML代码表示的是可以开合的导航菜单。

	<button class="nav-menu-toggle">Toggle Nav Menu</button>
	<nav>
		<ul>
			<li><a href="/">West Philadelphia</a></li>
			<li><a href="/cab">Cab Whistling</a></li>
			<li><a href="/throne">Throne Sitting</a></li>
		</ul>
	</nav>

下面这个是点击按钮之后控制导航菜单开合的javascript代码

	$('.nav-menu-toggle').on('click',function(){
		$('nav').toggle();
	});

这可能是最常用的实现方式。它能够使用，但是比较脆。javascript代码依赖了按钮的类名`nav-menu-toggle`。很可能在未来其他开发者或者健忘的你在重构代码的时候会删除或者重命名这个类名。

问题的核心是我们同时在表现和交互中使用了CSS的类名。这违反了关注点分离的原则，让维护更容易出错。

让我们用一个不同的方法来实现
	
	<button data-hook="nav-menu-toggle">Toggle Nav Menu</button>
	<nav data-hook="nav-menu">
		<ul>
			<li><a href="/">West Philadelphia</a></li>
			<li><a href="/cab">Cab Whistling</a></li>
			<li><a href="/throne">Throne Sitting</a></li>
		</ul>
	</nav>

这次我们使用这个data属性（data-hook）来选择元素。任何对CSS类的改变将不会影响到javascript，让我们能够实现关注点分离以及更加稳定的代码。

下面我们用`data-hook`属性来选择对应的元素：

	$('[data-hook="nav-menu-toggle"]').on('click',function(){
		$('[data-hook="nav-menu"]').toggle();
	});

需要注意的是，我也使用`data-hook`作为`nav`元素的选择器。你不一定需要，但是我喜欢这里面包含的思想：任何使用你看到`data-hook`，你会知道这个元素在javascript中引用到啦。

## 一些语法糖

我必须承认`data-hook`选择器并不是很漂亮。让我们通过扩展jQuery实现一个自定义的函数：

	$.extend({
		hook:function(hookName){
			var selector;
			if(!hookName || hookName === '*'){
				// select all data-hooks
				selector='[data-hook]'
			}else{
				// select specific data-hook
				selector='[data-hook*="'+hookName+'"]';
			}
			return $(selector);
		}
	});

上面准备完毕，我们来重写一下javascript。

	$.hook('nav-menu-toggle').on('',function(){
		$.hook('nav-menu').toggle();
	});

更好的是，我们甚至可以把一系列以空格分开的hook名字放在一个元素上。

	<button data-hook="nav-menu-toggle video-pause click-track">Toggle Nav Menu</button>

我们可以找到里面的任意个hook名字：
	$.hook('click-track'); // returns the button as expected

我们也能够找到页面上所有的hook元素

	// both are equivalent
	$.hook();
	$.hook('*');

## 防止函数表达式

到目前为止，我们在事件处理中使用的都是匿名函数。让我们重写一下，使用声明的函数来代替它。

	function toggleNavMenu(){
		$.hook('nav-menu').toggle();
	}

	$.hook('nav-menu-toggle').on('click',toggleNavMenu);

这让事件绑定的代码更加易读。这个`toggleNavMenu`函数名表达了意图，是代码自我注释的好例子。

我们同时也获得了可复用的能力，因为其他地方可能需要使用`toggleNavMenu`函数。

最后，这对于自动化测试来说是意见大喜事，因为声明的函数的单元测试要比匿名函数单元测试容易的多。

## 同时使用多个事件

jQuery提供了一个简单方便的语法来处理多事件的问题。比如，你可以为一系列空格隔开的事件列表绑定同一个事件处理函数。

	$.hook('nav-menu-toggle').on('click keydown mouseenter',trackAction);

如果你需要为不同的事件绑定不同的处理函数，你可以使用对象表达方式：
	$.hook('nav-menu-toggle').on({
		'click':trackClick,
		'keydown':tranckKeyDown,
		'mouseenter':trackMouseEnter
	});

反过来，你可以同时取消多个事件的绑定：
	
	// unbinds keydown and mouseenter
	$.hook('nav-menu-toggle').off('keydown mouseenter');

	// nuclear options:unbinds everything
	$.hook('nav-menu-toggle').off();

你可以想象到的是，不小心的取消事件绑定可能会导致严重的我们不想要的副作用。继续看我们可以通过哪些技巧来减轻这个问题。

## 小心的取消事件绑定

一般情况下我们不会在一个元素的同一事件类型绑定多个事件处理函数。让我们再看一下之前的那个按钮：
	
	<button data-hook="nav-menu-toggle video-pause click-track">Toggle Nav Menu</button>

不同的代码区域可能会在同一个元素的同一事件绑定不同的事件处理函数：

	// somewhere in the nav code
	$.hook('nav-menu-toggle').on('click',toggleNavMenu);

	// somewhere in the video playback code
	$.hook('video-pause').on('click',pauseCarltonDanceVideo);

	// somewhere in the analytics code
	$.hook('click-track').on('click',trackClick);

尽管我们使用了不同的选择器，但是这个元素现在有三个事件处理函数啦。假如我们的分析代码不在关心这个按钮：

	// no good
	$.hook('click-track').off('click');

糟糕的是，上面的代码实际上回删除所有的点击事件处理函数，不仅仅是`trackClick`。我们应该实用更加有辨别力的方式来指定我们需要删除的事件处理函数：
	
	$.hook('click-track').off('click',trackClick);

另一种方式是使用命名空间。任何事件都有资格使用一个命名空间来实现绑定和取消绑定，这样你就可以更好的控制事件绑定和取消绑定。

	// binds a click event in the "analytics" namespace
	$.hook('click-track').on('click.analytics', trackClick);

	// unbinds only click events in the "analytics" namespace
	$.hook('click-track').off('click.analytics');

你也可以使用多个命名空间：

	// binds a click event in both the "analytics" and "usability" namespaces
	$.hook('click-track').on('click.analytics.usability',trackClick);

	// unbinds any events in either the "analytics" OR "usability" namespaces
	$.hook('click-track').off('.usability .analytics');

	// unbinds any events in both the "analytics" AND "usability" namespaces
	$.hook('click-track').off('.usability.analytics');

需要注意的是，命名空间的顺序是没有关系的，因为命名空间不是层级式的。

如果你有一个复杂的功能需要多个元素绑定多个事件，那么使用命名空间是一种简单的把他们组织起来然后快速清除的方式：

	// free all elements on the page of any "analytics" event handling
	$('*').off('.analytics');

命名空间在写插件的时候尤其有用，因为这样你就能保证只会取消自己命名空间范围内的事件处理函数的绑定。