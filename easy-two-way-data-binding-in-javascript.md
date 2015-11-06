# 简单的用javascript实现的数据双向绑定

> **原文：**[Easy Two-Way Data Binding in Javascript](http://www.lucaongaro.eu/blog/2012/12/02/easy-two-way-data-binding-in-javascript/)

双向数据绑定指的是当对象的属性发生变化时能够同时改变对应的UI，反之亦然。换句话说，如果我们有一个`user`对象，这个对象有一个`name`属性，无论何时你对`user.name`设置了一个新值，UI也会展示这个新的值。同样的，如果UI包含一个用于数据用户名字的输入框，输入一个新值也会导致`user`对象的`name`属性发生相应的改变。

许多流行的javascript框架，像Ember.js,Angular.js或者KnockoutJS都会把双向数据绑定作为其中的主要特性来宣传。这并不意味着从头开始实现它很难，也不意味着当我们需要这种功能的时候，使用这些框架是我们唯一的选择。内部的潜在思想事实上是相当基础的，实现它可以归纳为以下三点：

1. 我们需要一种方式确定哪个UI元素绑定在哪个属性上。
2. 我们需要监控属性和UI的变化
3. 我们需要把所有绑定的对象和UI元素的变化传播出去。

尽管有好多种方式去实现这几点，一种简单高效的方法是我们通过发布订阅者模式来实现。方法很简单：我们可以使用定制的`data`属性作为HTML代码中需要绑定的属性。所有的绑定在一起的Javascript对象和DOM元素将会`订阅`这个发布订阅对象。任何时候我们检测到无论是Javascript对象亦或是HTML的input元素的变化，我们都是把事件代理传递给发布订阅对象，然后通过它把所有发生在绑定的对象和元素的的变化传递和广播出去。

## 一个用jQuery实现的简单例子

通过jQuery实现我们上面讨论的东西是相当简单明了的，因为作为一个流行的库，它让我们很简单的实现订阅和发布DOM事件，同时我们也可以定制一个：

	function DataBinder(object_id){
		// Use a jQuery object as simple PubSub
		var pubSub=jQuery({});

		// We expect a `data` element specifying the binding
		// in the form:data-bind-<object_id>="<property_name>"
		var data_attr="bind-"+object_id,
			message=object_id+":change";

		// Listen to chagne events on elements with data-binding attribute and proxy
		// then to the PubSub, so that the change is "broadcasted" to all connected objects
		jQuery(document).on("change","[data-]"+data_attr+"]",function(eve){
			var $input=jQuery(this);

			pubSub.trigger(message,[$input.data(data_attr),$input.val()]);
		});

		// PubSub propagates chagnes to all bound elemetns,setting value of
		// input tags or HTML content of other tags
		pubSub.on(message,function(evt,prop_name,new_val){
			jQuery("[data-"+data_attr+"="+prop_name+"]").each(function(){
				var $bound=jQuery(this);

				if($bound.is("")){
					$bound.val(new_val);
				}else{
					$bound.html(new_val);
				}
			});
		});
		return pubSub;
	}

至于javascript对象，下面是最小化的`user`数据模型实现的例子：

	function User(uid){
		var binder=new DataBinder(uid),
			
			user={
				attributes:{},
				// The attribute setter publish changes using the DataBinder PubSub
				set:function(attr_name,val){
					this.attributes[attr_name]=val;
					binder.trigger(uid+":change",[attr_name,val,this]);
				},

				get:function(attr_name){
					return this.attributes[attr_name];
				},
			
				_binder:binder
			};

		// Subscribe to PubSub
		binder.on(uid+":change",function(evt,attr_name,new_val,initiator){
			if(initiator!==user){
				user.set(attr_name,new_val);
			}
		});

		return user;
	}

现在，无论何时我们想要绑定一个对象的属性到UI上，我们只要在对应的HTML元素上设置合适的`data`属性。

	// javascript 
	var user=new User(123);
	user.set("name","Wolfgang");

	// html
	<input type="number" data-bind-123="name" />

input输入框上值得变化会自动的映射到`user`的`name`属性，反之亦然。大功告成！

## 不需要jQuery的实现方式

现在的大部分项目一般jQuery都已经在使用啦，所以上面的例子是完全可以接受的。但是如果我们需要完全不依赖jQuery，那么该怎么实现呢？好吧，事实上其实也不难办到（特别是当我们把对IE的支持只提供IE8以上的支持）。最后，我们只是要通过发布订阅者模式来观察DOM事件而已。

	function DataBinder( object_id ) {
  	// Create a simple PubSub object
  	var pubSub = {
        callbacks: {},

        on: function( msg, callback ) {
          this.callbacks[ msg ] = this.callbacks[ msg ] || [];
          this.callbacks[ msg ].push( callback );
        },

        publish: function( msg ) {
          this.callbacks[ msg ] = this.callbacks[ msg ] || []
          for ( var i = 0, len = this.callbacks[ msg ].length; i < len; i++ ) {
            this.callbacks[ msg ][ i ].apply( this, arguments );
          }
        }
      },

      data_attr = "data-bind-" + object_id,
      message = object_id + ":change",

      changeHandler = function( evt ) {
        var target = evt.target || evt.srcElement, // IE8 compatibility
            prop_name = target.getAttribute( data_attr );

        if ( prop_name && prop_name !== "" ) {
          pubSub.publish( message, prop_name, target.value );
        }
      };

 	// Listen to change events and proxy to PubSub
  	if ( document.addEventListener ) {
  	  document.addEventListener( "change", changeHandler, false );
  	} else {
    	// IE8 uses attachEvent instead of addEventListener
    	document.attachEvent( "onchange", changeHandler );
  	}

  	// PubSub propagates changes to all bound elements
  	pubSub.on( message, function( evt, prop_name, new_val ) {
    var elements = document.querySelectorAll("[" + data_attr + "=" + prop_name + "]"),
        tag_name;

    for ( var i = 0, len = elements.length; i < len; i++ ) {
      tag_name = elements[ i ].tagName.toLowerCase();

      if ( tag_name === "input" || tag_name === "textarea" || tag_name === "select" ) {
        elements[ i ].value = new_val;
      } else {
        elements[ i ].innerHTML = new_val;
      }
    }
  	});

 	 return pubSub;
	}

数据模型可以保持不变，除了在setter中对jQuery中`trigger`方法的调用，我们可以通过我们在PubSub中自定义的`publish`方法来代替。

	// In the model's setter:
	function User( uid ) {
  	// ...

 	 user = {
    	// ...
    	set: function( attr_name, val ) {
     	 	this.attributes[ attr_name ] = val;
      		// Use the `publish` method
      		binder.publish( uid + ":change", attr_name, val, this );
    	}
 	 }

  	// ...
	}

我们又一次通过一百行不到，又可维护的纯javascript完成了我们想要的结果。