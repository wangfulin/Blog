# 清楚地了解javascript中的`this`并且掌握它

> **原文：**[Understand Javascript's "this" With Clarity,and Master It](http://javascriptissexy.com/understand-javascripts-this-with-clarity-and-master-it/)

**(同时也学一下所有`this`最容易被误解的场景)**

Javascript中这个`this`关键词同时会让新手和有经验的人困惑。这篇文章的目的就是把它彻底地讲清楚。当我们看完这篇文章的时候，`this`会成为javascript当中我们从来不用担心的那部分啦。我们会懂得如何在每一个场景正确地使用`this`，包括在一些最让人迷惑最诡异的场景。

我们使用`this`的方式就像自然语言英语和法语使用主语一样。我们这样写：“约翰跑得很快，因为他想赶上火车。”注意主语`他`的使用。我们也能这样写：“约翰跑得很快，因为约翰想赶上火车。”我们在这种情况下一般不会重复使用`约翰`。因为如果我们这样做的话，我们的家人，朋友，同事会抛弃我们的。是的，他们会的。当然，也许不是你的家庭，但是像那些有钱有地位的朋友和同时会的。javascript中也会用这种优雅的方式，我们使用`this`关键词作为对对象的简称和引用。也就是说，场景的主题或者正在执行的代码的主题。考虑下面的例子：

	var person={
		firstName:"penelope",
		lastName:"Barrymore",
		fullName:function(){
			//Notice we use "this" as we used "he" in 
			//the example sentence earlier
			console.log(this.firstName+" "+this.lastName);
			// We could have also written this:
			console.log(person.firstName+" "+person.lastName);
		}
	}

如果我们使用`person.firstName`和`person.lastName`，就像上面的例子一样，我们的代码会变得不清晰。考虑到有可能有另外一个全局变量的名字是`person`（我们可能注意或者没注意到）。然后，对`person.firstName`的引用可能会尝试从`person`这个全局变量里面获取属性。这可能会导致很难debug的错误。所以我们使用`this`关键词不仅仅只是为了美感，而且也为了能够精确。它的使用事实上让我们的代码变得更加清晰，就好像主语`他`让我们的句子变得更清楚一样。它告诉我们我们正在指代句子开始特定的那个约翰。

就好像主语`他`被用来指代前面那个人一样，这个`this`关键词类似地也被用来指代这个函数所绑定的对象。这个`this`关键词不仅指代这个对象，同时也包含了这个对象的值。就好像这个主语一样，`this`被用来作为这个上下文的对象的指代。下面我们会了解到更多。


## Javascript的`this`关键词基础

首先，你要知道在javascript中所有的函数都是有属性的，就像对象有属性一样。并且，当函数执行的时候，它会获得这个`this`属性，
	
